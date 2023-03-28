# 如何为你的 PHP 应用程序创建一个多文件上传器

> 原文：<https://medium.com/geekculture/how-to-create-a-multiple-files-uploader-for-your-php-application-75c288d43521?source=collection_archive---------22----------------------->

![](img/82a1a2a2733282510c945b220c57db98.png)

Photo by [Christina @ wocintechchat.com](https://unsplash.com/@wocintechchat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/php?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如今，大多数开发人员使用 PHP 专门为他们的 web 或移动应用程序创建文件上传程序。例如，PHP 文件上传器是上传文件最有效的方法之一。

开发人员应该知道如何用 PHP 制作一个文件上传器来拥有更好的文件上传功能。

我们有这方面的知识，因此，它让你远远领先于你的同事。所以在学习如何创建 PHP 文件上传器之前，我们需要了解一些基础知识。

# 什么是 PHP

PHP，也称为超文本预处理器，是一个开源框架。大多数开发人员使用 PHP 是因为它是易于访问的服务器脚本语言。

PHP 是一种编程语言，即使新手也可以学习，因为它有极好的支持和可用的文档。此外，WordPress 用户可以使用 PHP 定制他们的网站。

# PHP 的用途是什么

同样，PHP 和其他任何框架一样，对开发者来说都是必不可少的。比如 PHP，灵活、直白，让初学者轻松使用。PHP 的其他用途包括:

*   它可以执行系统功能
*   创建、打开、写入、读取和打开文件
*   它可以控制表单
*   从文件中收集和保存数据
*   PHP 可以帮助添加、删除和更改数据库中的元素
*   PHP 允许你明确限制用户进入你网站的某些页面
*   最后，由于它的安全性，它可以加密存储的数据
*   既然我们知道了 PHP 的用途，我们现在可以继续如何创建一个文件上传 PHP。

# 如何用 PHP 制作文件上传器

开发者可以使用 Filestack 用 PHP 创建一个更好的文件上传器。首先，我们将考虑三个步骤。

# 创建 HTML 表单

首先，如果用户想要上传文件，您需要创建一个 HTML 表单供用户查看。类似地，为项目创建一个文件夹，并创建一个包含代码的 index.html，例如:

```
<!DOCTYPE html>
<html lang="en">
<head>
 <meta charset="UTF-8">
 <title>PHP File Upload</title>
</head>
<body>
 <form action="fileUploadScript.php" method="post" enctype="multipart/form-data">
     Upload a File:
     <input type="file" name="the_file" id="fileToUpload">
     <input type="submit" name="submit" value="Start Upload">
 </form>
</body>
</html>
```

代码的结果确实会创建 HTML 表单。在进入下一步之前，我们需要特别注意一些关于代码函数的事情，它们是:

```
action=“fileUploadScript.php”
```

这个代码标签确认负责处理后端文件上传的 PHP 脚本。

```
method=“post”
```

这个函数让浏览器知道在向服务器发送文件时使用哪种格式。

```
enctype=“multipart/form-data”
```

最后，这个函数决定表单可以提交的内容类型。

了解了这些代码功能后，我们可以进一步进行文件上传器的下一步。确保打开终端并从目录中启动服务器。例如，编写以下代码:

```
PHP-file-upload
```

此外，转到 web 浏览器并打开 localhost: 1234。如果它显示了上面的代码，那么您已经成功地启动了服务器。

# 让表单看起来更好

其次，设计表单样式，使其更适合用户交互。不幸的是，大多数开发人员经常忽视这条道路，因为他们觉得不值得这么做。

用 PHP 和 CSS 为你的文件上传器做这样的设计会让你的用户界面对用户更有吸引力。例如，您可以编写以下代码:

```
div.upload-wrapper {
  color: white;
  font-weight: bold;
  display: flex;
}input[type="file"] {
  position: absolute;
  left: -9999px;
}input[type="submit"] {
  border: 3px solid #555;
  color: white;
  background: #666;
  margin: 10px 0;
  border-radius: 5px;
  font-weight: bold;
  padding: 5px 20px;
  cursor: pointer;
}input[type="submit"]:hover {
  background: #555;
}label[for="file-upload"] {
  padding: 0.7rem;
  display: inline-block;
  background: #fa5200;
  cursor: pointer;
  border: 3px solid #ca3103;
  border-radius: 0 5px 5px 0;
  border-left: 0;
}label[for="file-upload"]:hover {
  background: #ca3103;
}span.file-name {
  padding: 0.7rem 3rem 0.7rem 0.7rem;
  white-space: nowrap;
  overflow: hidden;
  background: #ffb543;
  color: black;
  border: 3px solid #f0980f;
  border-radius: 5px 0 0 5px;
  border-right: 0;
}
```

这些代码的结果一定会让您的表单看起来更好。

# 文件上传程序 PHP

这一步是让你处理文件上传后端。首先，创建一个名为 uploads 的目录，以便保存文件。

其次，在目录 index.html 中创建另一个名为 fileUploadScript.php 的文件。完成后，编写以下代码；例如:

```
<?php
 $currentDirectory = getcwd();
 $uploadDirectory = "/uploads/";
 $errors = []; // Store errors here
 $fileExtensionsAllowed = ['jpeg','jpg','png']; // These will be the only file extensions allowed
 $fileName = $_FILES['the_file']['name'];
 $fileSize = $_FILES['the_file']['size'];
 $fileTmpName  = $_FILES['the_file']['tmp_name'];
 $fileType = $_FILES['the_file']['type'];
 $fileExtension = strtolower(end(explode('.',$fileName)));
 $uploadPath = $currentDirectory . $uploadDirectory . basename($fileName);

if (isset($_POST['submit'])) {
  if (! in_array($fileExtension,$fileExtensionsAllowed)) {
     $errors[] = "This file extension is not allowed. Please upload a JPEG or PNG file";
   }
  if ($fileSize > 4000000) {
     $errors[] = "File exceeds maximum size (4MB)";
   }
  if (empty($errors)) {
     $didUpload = move_uploaded_file($fileTmpName, $uploadPath);
    if ($didUpload) {
       echo "The file " . basename($fileName) . " has been uploaded";
     } else {
       echo "An error occurred. Please contact the administrator.";
     }
   } else {
     foreach ($errors as $error) {
       echo $error . "These are the errors" . "\n";
     }
   }
 }
?>
```

因此，在开始上传文件之前，编写以下代码以确保上传目录是可写的:

```
chmod 0755 uploads/
```

然后，通过运行以下命令，彻底检查您的 php.ini 以查看该文件是否适合处理文件:

```
PHP -ini
```

最后，启动 PHP 服务器并将其打开到 localhost:1234 以上传文件。一旦上传文件夹显示了保存的文件，您就成功地创建了一个文件上传器 PHP。

# 如何用 PHP 制作一个多文件上传器

你应该知道你可以用一个元素用 PHP 创建多个文件上传器。

开发人员可以专门设计您的单个[文件上传器 PHP](https://blog.filestack.com/content-cookbook/upload-files-to-your-site-by-url-in-php/) 代码，并允许文件元素选择多次。您可以采取以下步骤来创建多个文件上传程序。

# 确保您创建了一个 HTML 表单

首先，允许在

```
<form method='post' action='' enctype='multipart/form-data'>
 <input type="file" name="file[]" id="file" multiple>
 <input type='submit' name='submit' value='Upload'>
</form>
```

由于这些代码，您可以成功地创建 HTML 表单。

# 选择文件的依据

接下来，计算所有挑选的文件，并在整个文件上扭曲它们，以索引为基础选择文件。例如，编写以下代码来实现这一点:

```
<?php 
if(isset($_POST['submit'])){
 // Count total files
 $countfiles = count($_FILES['file']['name']);
 // Looping all files
 for($i=0;$i<$countfiles;$i++){
   $filename = $_FILES['file']['name'][$i];
 // Upload file
move_uploaded_file($_FILES['file']['tmp_name'][$i],'upload/'.$filename);
 }
} 
?>
```

因此，代码的结果将显示您已经成功地为您的 PHP 创建了一个多文件上传程序。

这里还刊登了[的](https://blog.filestack.com/thoughts-and-knowledge/make-file-uploader-php/)。