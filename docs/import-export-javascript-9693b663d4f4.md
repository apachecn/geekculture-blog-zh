# 导入-导出 JavaScript

> 原文：<https://medium.com/geekculture/import-export-javascript-9693b663d4f4?source=collection_archive---------20----------------------->

这是默认导入:

```
// B.js
import A from './A'
```

仅当`A`具有默认导出时才有效:

```
// A.js
export default 42
```

在这种情况下，导入时为其指定什么名称并不重要:

```
// B.js
import A from './A'
import MyA from './A'
import Something from './A'
```

因为它总是会解析为`A`的默认导出。

这是一种被命名为`A`的进口产品:

```
import { A } from './A'
```

仅当包含名为`A`的命名导出时，它才起作用:

```
export const A = 42
```

在这种情况下，名称很重要，因为您是通过导出名称导入特定的东西:

```
// B.js
import { A } from './A'
import { myA } from './A' // Doesn't work!
import { Something } from './A' // Doesn't work!
```

为了使这些工作，您将向`A`添加一个相应的命名导出:

```
// A.js
export const A = 42
export const myA = 43
export const Something = 44
```

一个模块只能有一个默认导出，但是可以有任意多个命名导出(零个、一个、两个或多个)。您可以一起导入它们:

```
// B.js
import A, { myA, Something } from './A'
```

这里，我们将默认导出作为导入，并将导出分别命名为 myA 和 Something。

```
// A.js
export default 42
export const myA = 43
export const Something = 44
```

我们还可以在导入时为它们指定不同的名称:

```
// B.js
import X, { myA as myX, Something as XSomething } from './A'
```

默认导出往往用于您通常期望从模块中获得的任何内容。命名导出倾向于用于可能很方便的实用程序，但并不总是必需的。然而，如何导出是由您决定的:例如，一个模块可能根本没有默认导出。 [JS 导入导出](https://i.stack.imgur.com/uCCXS.png)

有 4 种类型的出口。以下是每种用法的示例，以及一些使用它们的导入:

导出语法

```
// default exports
export default 42;
export default {};
export default [];
export default (1 + 2);
export default foo;
export default function () {}
export default class {}
export default function foo () {}
export default class foo {}// variables exports
export var foo = 1;
export var foo = function () {};
export var bar;
export let foo = 2;
export let bar;
export const foo = 3;
export function foo () {}
export class foo {}// named exports
export {};
export {foo};
export {foo, bar};
export {foo as bar};
export {foo as default};
export {foo as default, bar};// exports from
export * from "foo";
export {} from "foo";
export {foo} from "foo";
export {foo, bar} from "foo";
export {foo as bar} from "foo";
export {foo as default} from "foo";
export {foo as default, bar} from "foo";
export {default} from "foo";
export {default as foo} from "foo";
```

导入语法

```
// default imports
import foo from "foo";
import {default as foo} from "foo";// named imports
import {} from "foo";
import {bar} from "foo";
import {bar, baz} from "foo";
import {bar as baz} from "foo";
import {bar as baz, xyz} from "foo";// glob imports
import * as foo from "foo";// mixing imports
import foo, {baz as xyz} from "foo";
import foo, * as bar from "foo";// just import
import "foo";
```

# 走向

我很不情愿写这篇文章。

当你不熟悉设置过程时，保持简单，不要要求完美。迭代地精炼过程会让你更快地达到目标。如果你找到了一个可以帮助人们的解决方案，请在回复中留言。

另一个挑战是更新这些信息。如果您发现过时的信息，请列出旧的描述，并说明什么应该是新的。那将对我了解这些变化有很大帮助。

如果你知道更好的做事方法，也请告诉我。保持事情简单。我想应用 80/20 法则:涵盖重要但不是每个角度。