# 如何在 Laravel 上传视频

> 原文：<https://medium.com/geekculture/how-to-upload-a-video-in-laravel-1dc07bde3839?source=collection_archive---------0----------------------->

![](img/27f02c7fff24a23ee93d1f5c6f45298c.png)

Photo by [Ben Collins](https://unsplash.com/@bencollins?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

许多网站和在线系统要求用户将视频上传到互联网。这种特征常见于社交媒体网站、新闻网站、电子学习网站、在线杂志等。

许多 web 开发人员发现很难在网站上实现这个特性。然而，像 laravel 这样的 web 框架可以更容易地在网站上实现视频上传。在这篇文章中，我将一步一步地向你展示如何使用 laravel 上传一个 mp4 视频。

**步骤 1:** 使用 composer 创建一个新的 Laravel 项目。你可以给它取任何名字，但在这种情况下，我们将把它命名为视频*应用*。

```
composer create-project --prefer-dist laravel/laravel videoApp
```

**步骤 2:** 在 *videoApp* 项目中创建一个迁移来创建一个 *videos* 表。

```
cd videoApp
php artisan make:migration create_videos_table --create=videos
```

**步骤 3:** 在迁移中添加字段*视频*和*标题。*

```
<?phpuse Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;*class* CreateVideosTable *extends* *Migration* {
  *public* *function* up()
  {
    Schema::create('videos', *function* (Blueprint $table) {
      $table->bigIncrements('id');
      $table->string('title');
      $table->string('video');
      $table->timestamps();
  });
  } *public* *function* down()
  {
    Schema::dropIfExists('videos');
  }
}
```

**第四步:**接下来，创建一个名为 *Video 的模型。*

```
php artisan make:model Video
```

**第五步:**在模型中添加可填充数组中的*视频*字段和*标题*字段。

```
<?phpnamespace App;use Illuminate\Database\Eloquent\Model;*class* Video *extends* *Model* {
  *protected* $table = 'videos'; *protected* $fillable = [
      'title', 'video'
  ];}
```

**第六步:**在 ***config/*** 文件夹中，在一个名为*filesystems.php 的文件中添加一个名为 *my_files* 的新盘。*

```
'disks' => [
   'local' => [
      'driver' => 'local',
      'root' => storage_path('app'),
   ], 'public' => [
      'driver' => 'local',
      'root' => storage_path('app/public'),
      'url' => env('APP_URL').'/storage',
      'visibility' => 'public',
   ], 's3' => [
      'driver' => 's3',
      'key' => env('AWS_ACCESS_KEY_ID'),
      'secret' => env('AWS_SECRET_ACCESS_KEY'),
      'region' => env('AWS_DEFAULT_REGION'),
      'bucket' => env('AWS_BUCKET'),
      'url' => env('AWS_URL'),
   ], 'my_files' => [
      'driver' => 'local',
      'root'   => public_path() . '/'
   ]
],
```

**第五步:**接下来，创建一个控制器，命名为 VideoController。

```
php artisan make:controller VideoController
```

**第六步:**给*视频控制器*添加一个名为*上传视频的功能。*

```
<?phpnamespace App\Http\Controllers\Admin;use Illuminate\Http\Request;use App\Http\Controllers\Controller;
use Illuminate\Support\Facades\DB;
use App\Video;
use Storage;*class* VideoController *extends* *Controller* {
   *public* *function* uploadVideo(Request $request)
   {
      *$this*->validate($request, [
         'title' => 'required|string|max:255',
         'video' => 'required|file|mimetypes:video/mp4',
   ]); $video = new Video;
   $video->title = $request->title; if ($request->hasFile('video'))
   {
     $path = $request->file('video')->store('videos', ['disk' =>      'my_files']); $video->video = $path;
   }
   $video->save();

  }}
```

**第七步:**最后在*web.php*文件中的 routes 文件夹中

```
<?phpuse Illuminate\Http\Request;Route::post('uploadVideo', 'VideoController@uploadVideo')->name('videos.uploadedVideo');
```

步骤 8: 创建一个名为*uploadVideo.blade.php 的文件。*

```
<!DOCTYPE *html*>
<html>
<head>
 <meta *charset*="utf-8">
 <meta *name*="viewport" *content*="width=device-width, initial-scale=1,   shrink-to-fit=no">
<title>Video upload</title>
</head><body><div>
<h3>Upload a video</h3>
<hr><form *method*="POST" *action*="{{ route('videos.uploadVideo') }}" *enctype*="multipart/form-data" >
{{ csrf_field() }}
<div >
<label>Title</label>
<input *type*="text" *name*="title" *placeholder*="Enter Title">
</div><div >
<label>Choose Video</label>
<input *type*="file"  *name*="video">
</div><hr><button *type*="submit" >Submit</button></form></div></body></html>
```

现在我们有了。我希望你已经发现这是有用的。我会带着更多有趣的文章回来。感谢您的阅读。