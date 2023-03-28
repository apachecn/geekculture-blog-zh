# 如何在 FLUTTER 中将图像发布到 LARAVEL API

> 原文：<https://medium.com/geekculture/how-to-post-an-image-to-laravel-api-in-flutter-d15a25679c7c?source=collection_archive---------2----------------------->

![](img/d5ea51d72d9175a045860655bea6c007.png)

Photo by [Rahul Chakraborty](https://unsplash.com/@hckmstrrahul?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

大多数在线注册表单要求用户上传图像。对于一些开发人员来说，在移动应用程序中添加图片上传功能是一项挑战。本文将一步步向你展示如何将你的 FLUTTER 应用程序上的图片上传到一个 LARAVEL RESTFUL API。

本文分为两部分。

*   拉勒韦尔 API
*   颤振应用

第一部分。拉里韦尔 API

**步骤 1:** 使用 composer 创建一个新的 laravel 项目。你可以给它取任何名字，但是在这种情况下，我们将把它命名为 *ImageApi* 。

```
composer create-project --prefer-dist laravel/laravel ImageApi
```

**第二步:**在已创建的项目中。进行迁移以创建一个*图像*表。

```
php artisan make:migration create_images_table --create=images
```

**步骤 3:** 在迁移中添加图像字段*标题*和 *url。*

```
<?phpuse Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;class CreateImagesTable extends Migration
{

    public function up()
    {
        Schema::create('images', function (Blueprint $table) {
            $table->increments('id');
            $table->string('title');
            $table->string('url');
            $table->timestamps();
        });
    }public function down()
    {
        Schema::drop('images');
    }
}
```

**步骤 4:** 接下来，创建一个名为 Image 的模型

```
php artisan make:model Image
```

**第五步:**在模型中添加*可填充*数组中的*标题*字段和 *url* 字段。

```
<?phpnamespace App;use Illuminate\Database\Eloquent\Model;class Image extends Model
{
    protected $table = 'images';protected $fillable = [
        'title', 'url'
    ];
}
```

**第六步:**接下来，创建一个控制器，命名为 ImageController。

```
php artisan make:controller ImageController
```

**第七步:**到 ImageController。添加一个名为 *addimage 的函数。*

```
<?phpnamespace App\Http\Controllers;use App\Image;
use App\Http\Controllers\Controller;class ImageController extends Controller
{

    public function addimage(Request $request)
    { $image = new Image; $image->title = $request->title;

            if ($request->hasFile('image')) {

            $path = $request->file('image')->store('images'); $image->url = $path;
           } $image->save(); return new ImageResource($image);
    }
}
```

**第八步:**最后在*api.php*文件中的 routes 文件夹中

```
<?phpuse Illuminate\Http\Request;Route::resource('imageadd', 'Api\ImageController@addimage');
```

这将创建一个 Api 端点

> <domain-name>/api/imageadd</domain-name>

Flutter 应用程序将访问这个 API 端点。

**第二部分:颤振 APP**

**步骤 1:** 使用该命令创建一个新的 flutter App。

```
$ flutter create Imageapp
$ cd Imageapp
```

**第二步:**使用这个命令将[***image _ picker***](https://pub.dev/packages/image_picker)*包添加到你的 flutter 中*

```
*$ flutter pub add image_picker*
```

***第三步:**接下来，使用此命令将颤振*[***http***](https://pub.dev/packages/http)*包添加到您的项目中***

```
*****$** flutter pub add http***
```

*****第四步:**在颤振 App 的 lib 文件夹中。创建两个额外的 dart 文件。***

*   ***第一个将是 *service.dart* 。***
*   **第二个会是 *image_upload.dart* 。**

****第五步: *service.dart* 文件中的**。将 HTTP 包和 *dart:convert package* 导入这个文件。**

```
**import 'package:http/http.dart' as http;import 'dart:convert';**
```

**步骤 6: 创建一个名为 *Service* 的类，并在其中添加一个名为 *addImage 的函数。*这个函数将向我们在本文第 1 部分中创建的 laravel 端点发出 HTTP POST 请求。**

```
**import 'package:http/http.dart' as http;import 'dart:convert'; class Service {Future<bool> addImage(Map<String, String> body, String filepath) async {
    String addimageUrl = '<domain-name>/api/imageadd';
    Map<String, String> headers = {
      'Content-Type': 'multipart/form-data',
    };var request = http.MultipartRequest('POST', Uri.parse(addimageUrl))
      ..fields.addAll(body)
      ..headers.addAll(headers)
      ..files.add(await http.MultipartFile.fromPath('image', filepath));var response = await request.send();
    if (response.statusCode == 201) {
      return true;
    } else {
      return false;
    }
  }}**
```

****第七步: *image_upload.dart* 文件中的**。创建一个名为 ImageUpload 的有状态小部件**

```
**class ImageUpload extends StatefulWidget {
  ImageUpload({Key key}) : super(key: key);@override
  _ImageUploadState createState() => _ImageUploadState();
}class _ImageUploadState extends State<ImageUpload> {
  @override
  Widget build(BuildContext context) {
    return Container(

    );
  }
}**
```

****第八步:**导入***dart:io***库、 *image_picker 包*和*材质设计包*。另外，导入您创建的 *service.dart* 文件。******

```
******import 'dart:io';
import 'package:flutter/material.dart';
import 'package:image_picker/image_picker.dart';
import 'service.dart';******
```

********第 9 步:**在 ImageUploadState 类内部。创建以下变量。******

```
******Service service = Service();
final _addFormKey = GlobalKey<FormState>();
final _titleController = TextEditingController();

File _image;
final picker = ImagePicker();******
```

********第十步:**接下来，添加一个异步函数，这个函数将启动手机的图像库。调用这个函数 *getImage* 。******

```
******Future getImage() async {
    final pickedFile = await picker.getImage(source: ImageSource.gallery);
setState(() {
      if (pickedFile != null) {
        _image = File(pickedFile.path);
      } else {
        print('No image selected.');
      }
    });
  }******
```

********步骤 11:** 创建另一个函数，将选中的图像显示给用户。称之为 buildImage。******

```
******Widget _buildImage() {
    if (_image == null) {
      return Padding(
        padding: const EdgeInsets.fromLTRB(1, 1, 1, 1),
        child: Icon(
          Icons.add,
          color: Colors.grey,
        ),
      );
    } else {
      return Text(_image.path);
    }
  }******
```

********步骤 12:** 在主*构建*函数中添加一个 *TextFormField* 小部件。这是为了图像的标题。******

```
******Widget build(BuildContext context) {

  Container(
    child: Column(
      children: <Widget>[
       Text('Image Title'),
         TextFormField(
            controller: _titleController,
            decoration: const InputDecoration(
              hintText: 'Enter Title',),
            validator: (value) {
              if(value.isEmpty) {
                 return 'Please enter image title';}
                     return null;
                        },),
                        ],
                  ),
            ),
      );
  }******
```

********第 13 步:**接下来，添加一个运行 *getImage* 函数的按钮******

```
******Container(
    child: OutlineButton(
       onPressed: getImage, 
             child: _buildImage())),******
```

********第 14 步:**然后添加一个按钮，将标题和图像发送到服务器******

```
******Container(
     child: Column(
        children: <Widget>[
          RaisedButton(
            onPressed: () {
              if (_addFormKey.currentState.validate()) {
                _addFormKey.currentState.save();
                  Map<String, String> body = {
                   'title': _titleController.text,};
                    service.addImage(body, _image.path);
                     Navigator.pop(context);}},
            child: Text('Save',
               style: TextStyle(color: Colors.white)),
               color: Colors.blue,)],
                     ),
            )******
```

********第十五步:**最终的 *image_upload.dart* 文件将会是这样的******

```
******import 'dart:io';
import 'package:flutter/material.dart';
import 'package:image_picker/image_picker.dart';
import 'service.dart';class ImageUpload extends StatefulWidget {
  PostCreate();@override
  _ImageUploadState createState() => _ImageUploadState();
}class _ImageUploadState extends State<ImageUpload> {
  _ImageUploadState();

  Service service = Service();final _addFormKey = GlobalKey<FormState>();
  final _titleController = TextEditingController();
  File _image;
  final picker = ImagePicker();Future getImage() async {
    final pickedFile = await picker.getImage(source: ImageSource.gallery);setState(() {
      if (pickedFile != null) {
        _image = File(pickedFile.path);
      } else {
        print('No image selected.');
      }
    });
  }@override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Add Images'),
      ),
      body: Form(
        key: _addFormKey,
        child: SingleChildScrollView(
          child: Container(
            child: Card(
              child: Container(
                child: Column(
                  children: <Widget>[ Container(
                  child: Column(
                    children: <Widget>[
                      Text('Image Title'),
                      TextFormField(
                        controller: _titleController,
                        decoration: const InputDecoration(
                           hintText: 'Enter Title',),
                        validator: (value) {
                          if (value.isEmpty) {
                            return 'Please enter title';
                             }
                            return null;
                                },
                              ),
                            ],
                          ),
                        ),

              Container(
                child: OutlineButton(
                  onPressed: getImage, 
                    child: _buildImage())), Container(
                 child: Column(
                   children: <Widget>[
                     RaisedButton(
                       onPressed: () {
                         if(_addFormKey.currentState.validate()) {
                           _addFormKey.currentState.save();
                           Map<String, String> body = {
                            'title': _titleController.text};
                          service.addImage(body, _image.path);}},
                       child: Text('Save'),

                              )
                            ],
                          ),
                        ),
                      ],
                    ))),
          ),
        ),
      ),
    );
  }Widget _buildImage() {
    if (_image == null) {
      return Padding(

        child: Icon(
          Icons.add,
          color: Colors.grey,
        ),
      );
    } else {
      return Text(_image.path);
    }
  }
}******
```

********步骤 16:*main . dart*文件中的**导入 *image_upload.dart* 文件。然后，添加 ImageUpload 小部件。******

```
******import 'package:flutter/material.dart';import 'image_upload.dart';void main() {

  runApp(App());
}class App extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(

          child: MaterialApp(
        title: 'Flutter Demo',
        theme: ThemeData(
          primarySwatch: Colors.blue,
        ),
        home: ImageUpload(),
      ),
    );
  }
}******
```

******最后，测试应用程序。******

******现在我们有了。我希望你已经发现这是有用的。我会带着更多有趣的文章回来。感谢您的阅读。******