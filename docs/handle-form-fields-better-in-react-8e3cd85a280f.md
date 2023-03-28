# 在 react 中更好地处理表单字段

> 原文：<https://medium.com/geekculture/handle-form-fields-better-in-react-8e3cd85a280f?source=collection_archive---------13----------------------->

你最近是否写了很多处理输入字段的方法。

您并不孤单，我发现为每个字段编写句柄更改方法是一项重复而单调的任务。

如果你最近一直在这样做，请继续阅读。一个简单的技巧可以节省很多时间，并帮助你在现实世界中保持理智。

## 问题

考虑一个简单的注册表单，这里有一个界面表示这个表单的每个字段

```
interface RegisterForm {
  name: string;
  password: string;
  email: string;
  confirmpassword: string;
}
```

因此，如果我们对每个字段采用简单的方法，我们需要有状态变量和相应的`hanldeChange` 方法。

```
const [name, setName] = useState(''); // state variable// input field for name           <Input  
            type="text"
            placeholder="Enter Your Name"
            onChange={handleNameChange}
            value={name} />// method to handle name input field change
const handleNameChange = (e) => {
    setName(e.target.value);
  };
```

想象一下，注册表中的所有 4 个字段都是这样做的。这会很快失去控制。

## 解决办法

*   创建单个状态变量来保存表单状态

```
const [form, setform] = useState<RegisterForm>({
    name: "",
    email: "",
    password: "",
    confirmpassword: "",
  });
```

*   创建一个简单的通用`handleChange`方法，可以处理所有的输入字段。

***这里发生了什么，我们正在根据输入的 name 属性更新表单状态(上面创建的)中的正确字段。***

```
const handleChange = (e) => {
    setform({ ...form, [e.target.name]: e.target.value });
  };
```

*   为每个输入字段赋予唯一的名称属性，这样才能工作。名称应该与您的州定义中的名称相同。

```
// name input field
<Input  
            name="name"
            type="text"
            placeholder="Enter Your Name"
            onChange={handleChange}
            value={form.name}/>// email input field<Input  
            name="email"
            type="email"
            placeholder="Enter Your Email"
            onChange={handleChange}
            value={form.email}/>---- keep on adding as many fields as your want
```

就这样，从噩梦中挣脱出来，节省几分钟放松和啜饮咖啡。

编码快乐！！！