# React 中的受控表单

> 原文：<https://medium.com/geekculture/controlled-forms-in-react-ae9092433c08?source=collection_archive---------27----------------------->

受控表单是 React 类组件内部的表单，它使用状态在内部处理数据。使用输入字段中的 value 属性和事件处理程序，我们可以更新组件的状态。要开始创建一个受控表单，您需要构建您的类组件，因为我们将使用状态，而且功能组件没有状态。

所以，让我们从构建我们的类组件开始。确保在文件的开头导入 React 库。此外，确保定义类组件并从 React.Component 继承它。不要忘记导出默认组件！这将允许我们将这个组件导入到另一个组件中，并正确地呈现它。

```
import React from 'react'class Component extends React.Component {
}export default Component
```

当然，一个类组件需要一个 render 方法来呈现任何东西，所以让我们添加一个 render 方法。render 方法必须包含 return 语句，并且应该只返回一个对象。要返回多个 JSX 标签，您必须将它们全部包装在一个 div 标签中，或者如果您不想创建 div，您可以使用< > >作为占位符。

```
import React from 'react'class Component extends React.Component { render() {
    return(
      <div> </div> )
  }
}export default Component
```

下一步是在构造函数方法中定义我们的状态。确保在构造函数中包含 super()，这样我们就可以从 React.Component 继承方法。在花括号内，我们可以将初始状态设置为我们想要的任何值。我用 text 键设置我的状态，并将值设置为一个空字符串。

```
import React from 'react'class Component extends React.Component { constructor() {
    super()
    this.state = {
      text: ''
    }
  } render() {
    return(
      <div> </div>
    )
  }
}export default Component
```

一旦我们设置了组件的初始状态，我们就可以开始构建表单了。在 return 语句中的 div 标记内，我创建了一个表单标记，其中一个输入类型为 text，另一个输入类型为 submit。一旦我们建立了表单，我们必须向文本输入添加一个属性，以便输入值绑定到状态中的文本键。我们可以通过使用 value={}来实现这一点。在花括号内，我们必须使用 this.state.text 定义相应的状态。

```
import React from 'react'class Component extends React.Component { constructor() {
    super()
    this.state = {
      text: ''
    }
  } render() {
    return(
      <div>
        <form>
          <input type="text" value={this.state.text}/>
          <input type="submit"/>
        </form>
      </div>
    )
  }
}export default Component
```

下一组是当文本被输入到输入字段时即时更新我们的状态。我们可以通过使用 onChange 事件来做到这一点。因此，首先让我们构建 handleOnChange 方法，该方法允许我们在输入字段中键入文本时即时更新状态。我将这个函数创建为一个箭头函数，以便 this 关键字被正确绑定。handleOnChange 必须将事件作为我命名为 e 的参数，我们必须传入事件，因为这是一个事件处理程序，我们需要获取事件目标的值。目标的值将是输入到输入字段中的任何内容。一旦 handleOnChange 方法构建完成，我们就可以将它连接到文本输入。通过使用 onChange={}事件并以 this.handleOnChange 的形式传入我们的 handleOnChange 方法，我们能够立即更新我们的状态，因为当我们在输入字段中键入内容时会发生变化。

```
import React from 'react'class Component extends React.Component { constructor() {
    super()
    this.state = {
      text: ''
    }
  } handleOnChange = (e) => {
    this.setState({
      text: e.target.value}
    )
  } render() {
    return(
      <div>
        <form>
          <input type="text" value={this.state.text} onChange=            {this.handleOnChange}/>
          <input type="submit"/>
        </form>
      </div>
    )
  }
}export default Component
```

处理完 onChange 事件后，我们可以继续处理 onSubmit 事件。该事件将允许我们获取现在已修改的状态，并呈现它或在现在已修改的状态上运行方法。我们首先构建 handleOnSubmit 函数。该函数也将事件作为参数。传入该事件后，我们可以对该事件调用 preventDefault()，这样当我们提交表单时页面就不会刷新。然后，我们必须通过在表单标记中使用 onSubmit={}将表单连接到 handleOnSubmit 方法。然后，我们将 handleOnSubmit 方法作为 this.handleOnSubmit 传递给 OnSubmit 事件。如果您想在提交表单后清除输入字段和状态，我们可以再次使用 this.setState，方法是在构造器中以完全相同的方式重新定义状态，键为文本，值为空字符串。

```
import React from 'react'class Component extends React.Component { constructor() {
    super()
    this.state = {
      text: ''
    }
  } handleOnChange = (e) => {
    this.setState({
      text: e.target.value}
    )
  } handleOnSubmit = (e) => {
    e.preventDefault()
    alert('Text was just submitted: ' + this.state.text)
    this.setState({
      text: ''}
    )
  } render() {
    return(
      <div>
        <form onSubmit={this.handleOnSubmit}>
          <input type="text" value={this.state.text} onChange=            {this.handleOnChange}/>
          <input type="submit"/>
        </form>
      </div>
    )
  }
}export default Component
```

所以，这就是你如何建立一个可控的形式。同样，我们使用受控的表单和状态来处理组件内部的数据。组件内的状态充当由组件呈现的输入元素的数据源。使用 value 属性和事件处理程序，我们可以根据输入到输入字段中的内容来反映状态变化。这就是为什么一个表单是可控的。

如果不使用受控表单，我们将不得不使用存储在 DOM 中的表单，并使用对 DOM 节点的引用来从 DOM 中检索值。