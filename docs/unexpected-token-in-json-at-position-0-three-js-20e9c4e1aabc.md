# 意外标记< in JSON at position 0 Three.js

> 原文：<https://medium.com/geekculture/unexpected-token-in-json-at-position-0-three-js-20e9c4e1aabc?source=collection_archive---------0----------------------->

![](img/42bdc91e6cc2d5d3e11892c3dbcf78d4.png)

# Error

SyntaxError: Unexpected token < in JSON at position 0 Three.js

# Fix it

When you want to load a GLTF or OBJ or GLB or another object to your Three.js Project the error occurs in the HTTP request and return pure HTML

![](img/42847fd12413c7829c35e4cccc264aae.png)

Example XHR globe figure glb

![](img/301c4b33498e71ce2ec3486b820e6e61.png)

In this case returns react index.html

# Solution

The solution is in the loader, if you see I added another s on assets and the loader can’t find the figure in other cases is the root of the file or could be the name or on the root of the figure.

![](img/400d94ffec36c7f12df94355099c67f8.png)

Example where the error start

![](img/4f70f5dca682ce09c29658a0563593e2.png)

We have the 3D Figure

# Conclusion

This will help you to know where is the exact problem and how to fix it.

# Sources

[https://discourse . three js . org/t/syntax error-unexpected-token-in-JSON-at-position-0/13810/5](https://discourse.threejs.org/t/syntaxerror-unexpected-token-in-json-at-position-0/13810/5)