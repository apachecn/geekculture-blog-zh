# 我如何在 AWS Lambda 中实现六边形架构

> 原文：<https://medium.com/geekculture/how-i-implement-hexagonal-architecture-in-aws-lambda-469dad23727a?source=collection_archive---------15----------------------->

![](img/68350dd8d1d6b61741b64322d943a680.png)

Photo by [drmakete lab](https://unsplash.com/@drmakete?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 re:Invent 2021 上，我了解到了六边形建筑。我是一个好奇的人，所以我开始深入调查。像大多数架构概念一样，我找不到很多六边形架构实现的例子。我又看到了我自己的关于六边形建筑的文章，或者一些复制粘贴的内容农场的文章，这些文章从阿利斯泰尔·考克伯恩最初的文章中提取了一些流行词汇，并把它变成了他们自己的。

快进到本周，我在 YouTube 上偶然发现了一个名为[六角形建筑实例](https://www.youtube.com/watch?v=qZEMSK6S0QM)的视频。这是我最初寻找的，它激励我完成我对这个架构的理解。观看视频很有帮助，但他们提供的 GitHub repo 是我更好理解的捷径。对我来说，拥有相同代码的好版本和坏版本是一个关键。他们还使用了与我参加的 re:Invent talk 中演示的[代码库略有不同的名称。](https://github.com/aws-samples/aws-Lambda-hexagonal-architecture)

虽然我从未听说过`domains`、`ports`和`adapters`，但我确实知道这个新发现的回购协议中使用的`handlers`和`repositories`，这让我明白了我的第一点:命名大多是随意的，使用和上下文边界才是重要的。这两个代码库做了同样的事情，但是命名略有不同。想法是请求可以进入`adapter`，然后`adapter`通过`port`与业务逻辑`domain`通信。如果业务逻辑需要发起与其边界之外的任何东西的通信，那么它也将通过一个`port`与另一个`adapter`通信。

虽然我能够开始阅读和理解回购协议中的代码，并理解上下文边界所在，但我仍然有点困惑，为什么我们需要`port` s。两个回购的例子都表明`port` s 仅仅是一个传递，那么为什么不让`adapter`调用业务逻辑，而业务逻辑调用`adapter` s 呢？这个答案是我在软件工程界反复看到的。

是一个常用的缩写词，它给了我们 OOP 的原则。`D`代表[依赖倒置原则](https://www.digitalocean.com/community/conceptual_articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design#dependency-inversion-principle)，这意味着代码应该依赖抽象，而不是实现功能的代码。在六边形架构的上下文中，`port` s 是抽象，`adapter` s 和业务逻辑是实现功能的代码。一个`port`通常使用一个接口来实现，但是它在概念上是一个`adapter`和请求与响应的业务逻辑之间的契约，类似于一个 [API 模型](https://thomasstep.com/blog/api-gateway-models)。

带着我对六边形架构的新认识，我回顾了我过去的一些项目，想看看我离实现这个架构还有多远。事实证明，我差得不远。我们可以快速浏览一下较小的 Lambda，了解一下我曾经拥有的东西，然后我将重构我的代码以适应六边形架构，并突出显示我在此过程中所做的更改。简单来说，这段代码是我编写的日历 API 的 Lambda 处理程序。这个特定的端点在路径`/calendars/{calendarId}/events`下为日历创建一个事件，其中`{calendarId}`被用户替换为他们想要修改的先前创建的日历的 ID。

```
/**
 * File: index.js
 */

const events = require('/opt/database/events');
const { getDateRange } = require('/opt/getDateRange');
const { logger } = require('/opt/logger');

exports.handler = async function (event, context, callback) {
  try {
    const calendarId = event.pathParameters.calendarId;
    const body = JSON.parse(event.body);

    // Create Dates
    let startTime;
    let endTime;
    try {
      startTime = new Date(body.startTime);
      endTime = new Date(body.endTime);
    } catch (err) {
      logger.error('startTime and endTime are not ISO 8601 format.');
      const errorPayload = {
        statusCode: 400,
        body: JSON.stringify({
          errorMessage: 'startTime and endTime must be ISO 8601 timestamps e.g. 1995-12-13T03:24:00Z.',
        }),
      };
      return errorPayload;
    }

    // Check start comes after end
    if (!(startTime < endTime)) {
      logger.error('startTime is not earlier than endTime.');
      const errorPayload = {
        statusCode: 400,
        body: JSON.stringify({
          errorMessage: 'startTime must come before endTime.',
        }),
      };
      return errorPayload;
    }

    const dates = getDateRange(startTime, endTime);
    const eventId = await events.create(calendarId, dates, body);

    const data = {
      statusCode: 200,
      body: JSON.stringify({
        id: eventId,
      }),
    };
    return data;
  } catch (uncaughtError) {
    logger.error(uncaughtError);
    throw uncaughtError;
  }
}

/**
 * File: /opt/database/events.js
 */

const { documentClient } = require('./databaseSession');
const { generateToken } = require('../generateToken');

async function create(calendarId, dates, event) {
  const eventId = generateToken();
  const batchWritePayload = {};
  // Construct batchWritePayload
  await documentClient.batchWrite(batchWritePayload);

  return eventId;
}

module.exports = {
  create,
};
```

幸运的是，由于过去的经验，我已经将数据库功能从业务逻辑中完全分离出来。然而，数据库调用是实现功能的代码，所以我的业务逻辑应该通过抽象(`port`)与它通信，而不是直接通信。我的 Lambda 的业务逻辑也知道我的数据模型，知道我正在使用 DynamoDB，因为`getDateRange`调用只用于 NoSQL 数据库。(这是一个实现细节，我不认为这对理解重构是必要的。主要的一点是，我的业务逻辑知道特定于数据库的实现细节。)

我的传入`adapter`将会把特定于 Lambda 函数的调用转换成我的业务逻辑运行所需的参数，然后通过`port`与我的业务逻辑进行通信。目前，没有传入的`adapter`或`port`。

首先，让我们处理即将到来的`adapter`和`port`。

```
/**
 * File: index.js
 */

const { InputError } = require('/opt/errors');
const {
  GOOD_STATUS_CODE,
  BAD_INPUT_STATUS_CODE,
  SERVER_ERROR_STATUS_CODE,
} = require('/opt/config');
const { logger } = require('/opt/logger');

const { port } = require('./port');

exports.handler = async function (event, context, callback) {
  try {
    const calendarId = event.pathParameters.calendarId;
    const body = JSON.parse(event.body);

    const eventId = await port(calendarId, body);
    const data = {
      statusCode: GOOD_STATUS_CODE,
      body: JSON.stringify({
        id: eventId,
      }),
    };
    return data;
  } catch (err) {
    logger.error(err);
    let statusCode = SERVER_ERROR_STATUS_CODE;
    let message = 'Internal server error';

    if (err instanceof InputError) {
      statusCode = BAD_INPUT_STATUS_CODE;
      message = err.message;
    }

    const errorPayload = {
      statusCode,
      body: JSON.stringify({
        message,
      }),
    };

    return errorPayload;
  }
}

/**
 * File: port.js
 */

const { InputError } = require('/opt/errors');

const { logic } = require('./logic');

exports.port = async function (calendarId, payload) {
  // Validate input format
  try {
    startTime = new Date(payload.startTime);
    endTime = new Date(payload.endTime);
  } catch (err) {
    throw new InputError('startTime and endTime must be ISO 8601 timestamps e.g. 1995-12-13T03:24:00Z.');
  }

  // Construct event based on contract
  const event = {
    title: payload.title,
    startTime: payload.startTime,
    endTime: payload.endTime,
  };

  const eventId = await logic(calendarId, event);
  return eventId;
}
```

在进行重构的时候，我决定保持 Lambda 配置完全不变，只移动代码，因此传入的`adapter`将保持为`index.handler`。因为这是传入的`adapter`，所以它需要做的就是解析出`port`所需的参数，调用`port`，然后返回给客户端。这就是它所做的一切。用于清理这段代码的其他附加内容是`InputError`和`*_STATUS_CODE`常量。我希望能够抛出和处理自定义错误，并摆脱“幻数”

`port`可能是一个简单的传递，但是我也想在将参数抛给我的业务逻辑之前添加一点验证。旧的处理程序中存在`Date`创建验证。基于我与`logic`函数的契约构建`event`可能有点大材小用，因为在我的 API 网关中我也为这个端点设置了一个模型，但我会追求可重用性和适当的抽象。完全验证和构建`logic`的`event`确保了无论什么调用我的`port`抽象都无关紧要。这个`port`应该照顾到任何细节，从而成为一个适当的抽象。

我看的下一段代码是业务逻辑本身。

```
/**
 * File: logic.js
 */

const { InputError } = require('/opt/errors');
const { createEvent } = require('/opt/ports');
const { logger } = require('/opt/logger');

/**
 * Business logic
 * @param {string} calendarId
 * @param {Object} event
 * @param {string} event.title
 * @param {string} event.startTime ISO-8601 format
 * @param {string} event.endTime ISO-8601 format
 * @returns {string}
 */

exports.logic = async function (calendarId, event) {
  // Check that start comes before end
  const startTime = new Date(event.startTime);
  const endTime = new Date(event.endTime);
  if (!(startTime < endTime)) {
    throw new InputError('startTime must come before endTime.');
  }

  const eventId = await createEvent(calendarId, event);

  return eventId;
}
```

这里我可以将`Date`创建验证移出，因为`logic`现在知道它将被正确格式化的`event.startTime`和`event.endTime`调用。如果我使用 Typescript，我会创建一个`interface`。使用 Javascript 意味着我需要对自己多负责一点，并在自己的代码中执行契约。`logic`只处理特定于值的验证，因为它可以知道契约已经满足，我们可以从开始和结束时间比较中看到这一点。它也不再处理数据库实现细节，而是传递相关信息并让数据库`adapter`处理数据模型的构建。

最后，我重构了我的输出/数据库`port`和`adapter`。

```
/**
 * File: /opt/ports.js
 */

const events = require('/opt/database/events');
const { getDateRange } = require('/opt/getDateRange');

async function createEvent(calendarId, event) {
  const dates = getDateRange(startTime, endTime);
  const eventId = await events.create(calendarId, dates, event);
  return eventId;
}

module.exports = {
  createEvent,
};

/**
 * File: /opt/database/events.js
 */

const { documentClient } = require('./databaseSession');
const { generateToken } = require('../generateToken');

async function create(calendarId, dates, event) {
  const eventId = generateToken();
  const batchWritePayload = {};
  // Construct batchWritePayload
  await documentClient.batchWrite(batchWritePayload);

  return eventId;
}

module.exports = {
  create,
};
```

注意到什么了吗？`/opt/database/events.js`中的代码没有变化。正如我所说的，我幸运地正确抽象了数据库功能，问题是我的业务逻辑调用了数据库功能，而不是功能的抽象(数据库`port`)。这使得我的数据库`port`和`adapter`的重构变得简单。我需要做的就是将业务逻辑之外的`getDateRange`调用移到我的`port`中，并让我的`port`调用数据库`adapter`。

好了，我们有了。自从几个月前我参加了 re:Invent 的演讲后，我就一直想实现一个六边形架构，尤其是在 Lambda 函数中。在找到另外几个关于实现的关键资源后，我现在更有信心理解细节和界限。正如我在很多其他帖子中说过的，我在生活的很多方面都在使用改善。这只是我第一次实现六边形架构，所以要有所保留。我已经尽我所知实现了所有的事情，但是我确信在将来我会学到新的更好的方法。现在，我希望这能帮助其他人更快地理解实现。快乐编码。

*原载于 2022 年 1 月 27 日*[*【https://thomasstep.com】*](https://thomasstep.com/blog/how-i-implement-hexagonal-architecture-in-aws-lambda)*。*