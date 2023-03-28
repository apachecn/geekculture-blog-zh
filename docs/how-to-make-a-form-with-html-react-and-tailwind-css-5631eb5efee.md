# 如何用 HTML，React，和 Tailwind CSS 制作表单

> 原文：<https://medium.com/geekculture/how-to-make-a-form-with-html-react-and-tailwind-css-5631eb5efee?source=collection_archive---------26----------------------->

![](img/4ce9bdca461bcc53d87a5de107592730.png)

Photo by [Stefen Tan](https://unsplash.com/@stefentan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/keyboard?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在过去的一个月左右的时间里，我一直在开发一个新的服务，它以 API 的形式提供[认证，我开始需要用户在前端输入。如今，几乎每个网络应用都允许用户输入某种信息，然后用这些信息代表他们进行 API 调用。从输入表单中提取数据有时有点棘手。我的一个用例是允许用户在登录之前输入他们的凭证。](https://crowauth.com/)

我以前发布过关于[用材质 UI 组件](https://thomasstep.com/blog/making-a-form-in-material-ui-with-textfield-and-button)创建表单，但是我不想在这个项目中依赖材质 UI。相反，我转向使用简单的 HTML 和 Tailwind CSS 来设计我的样式。使用 React 从用户那里获取信息的想法与上一篇文章类似。对于一些 React 监听器，我使用了两个`input`和一个`button`。每当任何一个`input`字段改变时(这意味着用户输入的信息)，我就更新一个状态变量来跟踪数据。然后当用户点击`button`时，我已经将输入的信息存储在 state 中，供`button`的`onClick`处理程序使用。下面是基本的 HTML，但是我会马上添加一些顺风样式。

```
export default function SigninPage() {
  const [signInEmail, setSignInEmail] = useState('');
  const [signInPassword, setSignInPassword] = useState('');
  const [errorMessage, setErrorMessage] = useState('');

  function handleSignInEmailFieldChange(event) {
    event.preventDefault();
    setSignInEmail(event.target.value);
  }

  function handleSignInPasswordFieldChange(event) {
    event.preventDefault();
    setSignInPassword(event.target.value);
  }

  async function handleSignIn(event) {
    event.preventDefault();
    setErrorMessage('');

    try {
      // Sign In logic using signInEmail and signInPassword state
      setErrorMessage('There was an error signing in');
    } catch (err) {
      // Remediation logic
      setErrorMessage('There was an error signing in');
    }
  }

  return(
    <div>
      <label>Email address</label>
      <input
        type="email"
        placeholder=""
        value={signInEmail}
        onChange={(e) => handleSignInEmailFieldChange(e)}
      />
      <label>Password</label>
      <input
        type="password"
        placeholder=""
        value={signInPassword}
        onChange={(e) => handleSignInPasswordFieldChange(e)}
      />
      <button
        onClick={handleSignIn}
      >
        Sign In
      </button>
      <p>
        { errorMessage }
      </p>
    </div>
  );
}
```

这应该处理与获取和使用用户输入相关的所有逻辑，但是任何试图使用它的人可能都不会喜欢这个结果。我将添加一些顺风的样式，但是从这里开始，你可以随意使用你自己的样式。

```
export default function SigninPage() {
  const [signInEmail, setSignInEmail] = useState('');
  const [signInPassword, setSignInPassword] = useState('');
  const [loading, setLoading] = useState(false);
  const [errorMessage, setErrorMessage] = useState('');

  function handleSignInEmailFieldChange(event) {
    event.preventDefault();
    setSignInEmail(event.target.value);
  }

  function handleSignInPasswordFieldChange(event) {
    event.preventDefault();
    setSignInPassword(event.target.value);
  }

  async function handleSignIn(event) {
    event.preventDefault();
    setErrorMessage('');
    setLoading(true);

    try {
      // Sign In logic using signInEmail and signInPassword state
      setErrorMessage('There was an error signing in');
      setLoading(false);
    } catch (err) {
      // Remediation logic
      setErrorMessage('There was an error signing in');
      setLoading(false);
    }
  }

  return(
    <div className="flex flex-col items-center justify-center text-center">
      <div className="lg:w-2/5 md:w-3/5 w-4/5">
        <label className="mt-6">Email address</label>
        <input
          type="email"
          className="mt-1 w-full rounded-xl border-gray-300 shadow-sm focus:border-purple-500 focus:ring focus:ring-purple-500 focus:ring-opacity-50"
          placeholder=""
          value={signInEmail}
          onChange={(e) => handleSignInEmailFieldChange(e)}
        />
        <label className="mt-6">Password</label>
        <input
          type="password"
          className="mt-1 w-full rounded-xl border-gray-300 shadow-sm focus:border-purple-500 focus:ring focus:ring-purple-500 focus:ring-opacity-50"
          placeholder=""
          value={signInPassword}
          onChange={(e) => handleSignInPasswordFieldChange(e)}
        />
        <button
          className={`
            bg-white mt-6 border rounded-xl border-gray-300 p-2 hover:bg-purple-500 hover:text-white
            ${
              loading ? "bg-purple-500 text-white animate-pulse" : ""
            }
          `}
          disabled={loading}
          onClick={handleSignIn}
        >
          Sign In
        </button>
        <p className="text-red-900">
          { errorMessage }
        </p>
      </div>
    </div>
  );
}
```

最外层的样式`div`将一列中的所有内容居中对齐。第二个`div`对不同屏幕尺寸的字段进行宽度限制。给了`label`一家最大的利润。当`input`字段处于焦点时，它们的边缘周围会有一些颜色。包含`errorMessage`的最后一个`p`现在将以红色显示文本。最后，`button`被设计成当它被点击后悬停或聚焦时显示颜色。每当在`handleSignIn`内点击`button`时`loading`状态打开，每当登录逻辑完成时再次关闭。当页面等待登录逻辑时，按钮将保持彩色，并向用户显示幕后正在发生的事情。有关这些类的更多信息，请参考 [Tailwind 的文档](https://tailwindcss.com/docs)。