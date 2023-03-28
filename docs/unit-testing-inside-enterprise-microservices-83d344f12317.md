# 企业微服务内部的单元测试

> 原文：<https://medium.com/geekculture/unit-testing-inside-enterprise-microservices-83d344f12317?source=collection_archive---------26----------------------->

![](img/83793e00c1153025945c0bcb12cdb23b.png)

企业微服务往往是多种技术的结合。这篇文章的目的是讨论企业 JS 服务中常见的不同场景的多种方法。

这些场景:

1.  使用对企业 SOAP 服务的 XML 请求测试服务调用
2.  测试对企业服务的标准 POST 调用
3.  测试数据库连接和相关查询
4.  测试 LDAP 连接和相关搜索
5.  测试包含上述一项或多项的快速路线

在进入本文之前，您可以在我为此整理的 Github 资源库中找到大多数代码示例。

[https://github.com/goldsziggy/enterprise-testing-patterns](https://github.com/goldsziggy/enterprise-testing-patterns)

# 目标

问题仍然是，为什么要执行单元测试？在您的快速路线周围添加测试有助于确保您的完整代码路径和工具链得到测试。适当的代码覆盖保证您的调用仍然被调用，并且所需的业务逻辑不受阻碍。这让开发人员在更新依赖项时更有信心，并允许更多的自动化和更少的人工干预。

这个帖子不是什么。这篇文章并不是一个如何编写企业快速服务的好例子。这里的 repo I 链接不做任何输入清理，只是为了举例和展示半真实环境中的测试模式而编写的。

# 工具链

为了完成上述测试，我们将利用以下工具链/堆栈:

*   [Jest](https://jestjs.io/) —这将是我们的试运行和整体框架。在测试 MySQL 和 LDAP 响应时，我们将利用 Jest 模拟
*   一个非常简单的 HTTP 模仿库。每当我们需要模拟出站连接(REST/SOAP)时，都会用到这一点
*   [SuperTest](https://www.npmjs.com/package/supertest) —用于测试 express 应用服务器的 HTTP 库。

# 使用对企业 SOAP 服务的 XML 请求测试服务调用

`[https://github.com/goldsziggy/enterprise-testing-patterns/blob/master/__tests__/soap.spec.js](https://github.com/goldsziggy/enterprise-testing-patterns/blob/master/__tests__/soap.spec.js)`

当围绕基于网络的请求(如 SOAP)构建单元测试时，我们通常不关心传输层。这意味着，我们的测试目标应该是在发送请求之前验证我们的请求，以及在请求之后我们的服务如何表现。

也就是说，我们可以开始检查为这个例子编写的测试用例。

## 进口货

```
const nock = require('nock') // HTTP Mocking library
const parser = require('fast-xml-parser') // XML parser
const lodash = require('lodash') 
const { currencyConversion } = require('../src/soap') // the sample soap service call
```

`nock`和`fast-xml-parser`在我看来是必备的。正如我前面提到的，我们不关心 HTTP 传输层，所以我们可以依靠`nock`来模拟等式外的那部分，以使我们的测试用例一致和准确。`fast-xml-parser`将用于确认我们生成的请求 XML 满足某些业务需求。

## 样本响应

```
const sampleSuccessResponse = `<?xml version="1.0" encoding="utf-8"?>
<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Body>
    <GetCurrencyRateResponse >
      <GetCurrencyRateResult>15</GetCurrencyRateResult>
    </GetCurrencyRateResponse>
  </soap:Body>
</soap:Envelope>`const sampleErrorResponse = `<?xml version="1.0" encoding="utf-8"?><soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema"><soap:Body><soap:Fault><faultcode>soap:Client</faultcode><faultstring>Server was unable to read request. ---&gt; There is an error in XML document (12, 18). ---&gt; The string \'1603068090810\n      \' is not a valid AllXsd value.</faultstring><detail /></soap:Fault></soap:Body></soap:Envelope>`
```

Soap 响应通常比 REST 响应更冗长，响应中使用了不同的 XML 标记。正是出于这个原因，我建议将这些响应存储在一个变量中。在这个例子中，我将字符串硬编码到文件的顶部，但是作为一个更好的实践，我将这些响应放在 soap 请求所在的文件夹中。

## 测试类型 1:快乐之路

```
it('Should return the currency upon success', async () => {
  nock(soapURL)
    .post('/converter.asmx?WSDL')
    .reply(200, sampleSuccessResponse)
  const val = await currencyConversion(Date.now(), false)
  expect(val).toEqual(15)
})
```

快乐路径测试总是测试中最小的。这里我们简单地调用函数，并依靠`nock`来回复我们一致的 sampleSuccessResposne。遵循`nock` API，我们为我们的 soap 服务指定 baseURL，并告诉`nock`使用 200 成功代码和我们的示例成功响应来强制回复。

## 测试类型 2:验证动态请求

```
it('Should set currency to US when isUSD is true', async () => {
  expect.assertions(1)

  nock(soapURL)
    .post(`/converter.asmx?WSDL`)
    .reply(200, (uri, requestBody) => {
      const parsed = parser.parse(requestBody)
      const parsedRate = lodash.get(parsed, 'soap:Envelope.soap:Body.GetCurrencyRate')
      expect(parsedRate.Currency).toEqual('US')
    })

  await currencyConversion(Date.now(), true)})
```

在我们的示例企业服务中，我们的 soap 请求函数将基于动态模板构建一个 XML 字符串。上面的测试证实了动态生成的 XML 的正确性。为此，我们利用 nock 的能力在请求体上应用自省。您会注意到，在我们的 await 之后，我们利用 Jests `expect.assertions` API 来验证我们的断言是否被实际调用。这个调用出现在文件的开头，让 Jest 知道会发生什么。

## 测试类型 3:预期的错误情况

```
it('Should throw the soap Error when there is XML returned', async (done) => {
  nock(soapURL)
    .post('/converter.asmx?WSDL')
    .reply(500, sampleErrorResponse)

  try {
    await currencyConversion(Date.now(), false)
  } catch (e) {
    expect(e.message).toEqual(expect.stringContaining('Server was unable to read request'))
    return done()
  }
  done(new Error('expected the soap call to throw an error and none was recieved'))
})
```

上面的测试用例显示了我们使用预期的`errorResponseXML`对象强制执行 500 响应。我们的示例服务中的逻辑旨在使用从 XML 对象派生的正确消息创建一个新错误。该测试确保我们预期的错误消息存在。

# 测试对企业服务的标准 POST 调用

`[https://github.com/goldsziggy/enterprise-testing-patterns/blob/master/__tests__/services.spec.js](https://github.com/goldsziggy/enterprise-testing-patterns/blob/master/__tests__/services.spec.js)`

有时，为单个服务编写测试用例可能是一项有价值的任务。我在这里的理念是 rest 服务调用应该是具有很少功能的简单明了的函数。这些小函数将创建许多“样板”外观的测试。

## 进口货

```
const nock = require('nock')
const services = require('../src/services')
```

为了测试我们的小服务调用，我们只需要再次调用我们的`nock`库。我们将利用这个库来模拟成功、失败和超时，而不需要发出网络请求。下面，我们将简单地以不同的方式利用 nock 函数，使我们能够创建/导致错误

## 测试案例 1:快乐之路

```
it('Should return the data on a successful call', async () => {
  const id = '123'
  const mockData = { foo: 'bar' }

  nock('https://ghibliapi.herokuapp.com')
    .get(`/films/${id}`)
    .reply(200, mockData)

  const data = await services.getByFilmId(id, { info: jest.fn(), error: jest.fn() })
  expect(data).toEqual(mockData)
})
```

注意服务电话的线路，你会注意到我们利用笑话模仿。在这个例子中，我们的服务被传递了一个用于注销的 Bunyan 的子记录器。因为我们不在快速调用堆栈的中间，所以我们必须模拟该对象的实现，以免出错

## 测试用例 2:预期的失败

```
it('Should throw an error when the service is down', async () => {
  const id = '123'
  nock('https://ghibliapi.herokuapp.com')
    .get(`/films/${id}`)
    .reply(500, () => {})
  try {
    await services.getByFilmId(id, { info: jest.fn(), error: jest.fn() })
    throw new Error('The service call did not throw the expected error')
  } catch (e) {
    expect(e.message).toEqual('Request failed with status code 500')
  }
})
```

## 测试用例 3:超时行为

```
it('Should throw an error if the service call timesout', async (done) => {
  const id = '123'
  nock('https://ghibliapi.herokuapp.com')
    .get(`/films/${id}`)
    .delayConnection(1500)
    .reply(200, {})
  try {
    await services.getByFilmId(id, { info: jest.fn(), error: jest.fn() })
    throw new Error('The service call did not throw the expected error')
  } catch (e) {
    expect(e.message).toEqual('timeout of 1000ms exceeded')
  }
  done()
})
```

# 测试数据库连接和相关查询

作为测试实现代码的开发人员，没有必要调用底层数据库。在这些场景中，我们可以依靠 Jest 模拟来成为我们想象中的数据库连接。通过这种方式，我们可以完全控制输出，他们可以正确地预测成功和错误的情况。

## 设置

当构建数据库连接时，我建议使用单个文件来准备和导出我们的数据库客户机。然后，我们的服务可以在整个过程中利用这个文件，创建一个我们的请求发起的单点，从而简化测试。

```
const mysql = require('mysql2')const pool = mysql.createPool({
  host: process.env.MYSQL_HOST || 'example.org',
  user: process.env.MYSQL_USER || 'bob',
  password: process.env.MYSQL_PW || 'secret',
  database: process.env.MYSQL_DB || 'test',
})module.exports = pool.promise()
```

## 嘲笑和测试

`[https://github.com/goldsziggy/enterprise-testing-patterns/blob/master/__tests__/mysql.spec.js](https://github.com/goldsziggy/enterprise-testing-patterns/blob/master/__tests__/mysql.spec.js)`

```
const mockPool = require('../src/mysql/connection')jest.mock('../src/mysql/connection', () => {
  return {
    query: jest.fn(),
  }
})
```

我们的实现代码是导出的池连接，我们可以创建对象和查询函数的模拟。通过完成以上步骤，我们可以自由地操纵`mockPool`并为我们的测试用例伪造我们的返回结果。

```
it('Should run the query from inputOne', async (done) => {
  mockPool.query.mockImplementation((sql) => {
    // place your mock data here for testing more complex returns!
    return [1, 2]
  }) await querySomething(true) expect(mockPool.query).toHaveBeenCalledWith('SELECT * FROM inputOne WHERE 1=1')
  done()
})
```

在这个示例测试中，我们告诉我们的`mockPool`返回一个模拟数组。在数据设置之后，我们在`querySomething`中调用我们的功能逻辑。完成上述设置后，我们可以验证 SQL 查询是什么。更进一步，如果我们的`querySomething`函数执行额外的业务逻辑或数据操作，我们也可以通过 expect 子句确认函数的输出。

# 测试 LDAP 连接和相关搜索

模拟 LDAP 将遵循与上面的 MySQL 示例相同的模式。我们将创建一个文件作为所有 LDAP 连接/查询的中介——并将其用于模拟。当与 LDAP 交互时，我更喜欢`ldapts`包，因为它确实是有保证的。

```
const { Client } = require('ldapts')const url = 'ldaps://ldap.jumpcloud.com'const client = new Client({
  url,
})return client
```

## 嘲笑和测试

```
const { getUsers } = require('../src/ldap/ldap-calls')
const mockLdap = require('../src/ldap/client')jest.mock('../src/ldap/client', () => {
  return {
    bind: jest.fn(),
    unbind: jest.fn(),
    search: jest.fn(),
  }
})...afterEach(() => {
  mockLdap.search.mockReset()
  mockLdap.bind.mockReset()
  mockLdap.unbind.mockReset()
})
```

从上面，您会注意到 MySQL 部分中使用的相同模式。我们使用类似单例的客户机对象，并准备将其用作模拟对象。与 MySQL 实现不同，您会注意到我们对测试运行执行了清理`afterEach`。像这样的清理功能是一个很好的实践，有助于确保您的模拟不会产生副作用和创建不可靠的测试。

```
it('When LDAP succeeds should return the searchEntries, and call unbind', async (done) => {
  mockLdap.search.mockImplementation(() => {
    // place your mock data here for testing more complex returns!
    return { searchEntries: 'matt@zygowicz.com' }
  })
  try {
    const user = await getUsers('foo')
    expect(user).toEqual('matt@zygowicz.com')
    expect(mockLdap.bind).toHaveBeenCalled()
    expect(mockLdap.search).toHaveBeenCalled()
    expect(mockLdap.unbind).toHaveBeenCalled()
  } catch (e) {
    done(e)
  }
  done()
})
```

上面的测试案例展示了我们如何创建搜索功能的模拟实现。这个模拟只是返回一个硬编码的条目。这允许我们在代码库中确认成功或失败发生时的逻辑。此外，我确认我们的 LDAP 连接已经解除绑定，并在调用后关闭。

# 测试包含上述一项或多项的快速路线

最后，既然我们已经测试了所有单独的部分，我们需要确认我们绑定它们的逻辑是准确的。在本节中，我们将吸取上面的经验教训，从我们的微服务中模拟出一条单独的路由。

```
// same scaffolding we've seen before
...
  it("should call getAllFilms, taking the first result and passing it to getFilmById", (done) => {
    const mockItem = { id: "321" };
    mockLdap.search.mockImplementation(() => {
      return { searchEntries: "matt@zygowicz.com" };
    });
    nock("https://ghibliapi.herokuapp.com")
      .get(`/films`)
      .reply(200, [mockItem, { id: "123" }]);
    nock("https://ghibliapi.herokuapp.com")
      .get(`/films/${mockItem.id}`)
      .reply(200, mockItem);
    request(app)
      .post("/enterprise-testing-patterns")
      .expect(200)
      .end((err, res) => {
        if (err) {
          done(err);
        }
        expect(res.body).toEqual({
          film: { id: "321" },
          user: "matt@zygowicz.com",
        });
        done();
      });
  });
```

从这个测试的顶部到底部，你会注意到它非常简洁。大部分测试将模拟路由将使用的堆栈中的各个组件。这个测试的好处是它创建了所有组件如何集成的端到端视图

在我看来，这些类型的测试是最有价值的，因为一般来说，大多数业务规则都围绕着如何操作您正在实时检索的数据。然而，这些测试并非没有缺点；随着测试规模的增加，您引入人为剥落的可能性也会增加。