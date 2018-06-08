# KongzueTakePhoto
Kongzue APP拍照&相册选择工具

<a href="https://github.com/kongzue/KongzueTakePhoto/">
<img src="https://img.shields.io/badge/KongzueTakePhoto-2.0.1-green.svg" alt="Kongzue TakePhoto">
</a>
<a href="https://bintray.com/myzchh/maven/TakePhoto/2.0.1/link">
<img src="https://img.shields.io/badge/Maven-2.0.1-blue.svg" alt="Maven">
</a>
<a href="http://www.apache.org/licenses/LICENSE-2.0">
<img src="https://img.shields.io/badge/License-Apache%202.0-red.svg" alt="Maven">
</a>
<a href="http://www.kongzue.com">
<img src="https://img.shields.io/badge/Homepage-Kongzue.com-brightgreen.svg" alt="Maven">
</a>

### 引入TakePhoto到您的项目

引入方法：

Maven：
```
<dependency>
  <groupId>com.kongzue.takephoto</groupId>
  <artifactId>takephoto</artifactId>
  <version>2.0.1</version>
  <type>pom</type>
</dependency>
```

Gradle：
```
implementation 'com.kongzue.takephoto:takephoto:2.0.1'
```

### 说明
1) 在 Android 6.0 以上会自动申请权限，但依然需要您在您的项目中预先声明相机权限和存储读取、写入权限。申请权限的步骤会自动进行。因申请权限需要，您在调用本工具的 Activity 必须是继承自 AppCompatActivity 的，本工具采用单例方式进行使用，在 getInstance() 时必须传入 Activity extends AppCompatActivity.
2) 本工具仅提供默认的单图片拍摄以及相册中的单图片选择功能。
3) 本工具默认集成图片压缩的 CompressHelper 框架（ https://github.com/nanchen2251/CompressHelper ） 感谢 @nanchen2251 开源做出的贡献。
4) 本工具已经处理在 Android 7.0 以上时系统禁止 APP 互相传输 Uri 可能导致的无法正常调用相机拍摄照片存储在指定目录的问题。请勿担心此问题放心使用。
5) 本工具需要您提供的参数对照表如下：
6) 本框架中使用到的图像压缩框架是 @nanchen2251 的 CompressHelper: https://github.com/nanchen2251/CompressHelper 感谢他开源做的贡献。原压缩框架使用的开源协议为 Apache License 2.0，如要使用请遵守该协议。

图片压缩相关：

属性 | 含义 | 说明
---|---|---
DEFAULT_QUALITY | 图片质量 | 可选，默认值80（%）
DEFAULT_MAX_WIDTH | 图片最大宽度 | 可选
DEFAULT_MAX_HEIGHT | 图片最大高度 | 可选
DEFAULT_PIC_TYPE | 图片输出类型 | 可选（Bitmap.CompressFormat类型）

功能相关：

方法 | 含义 | 是否必须
---|---|---
doOpenCamera() | 调用相机拍照 | 可选
doOpenGallery() | 调用相册选择照片 | 可选
onActivityResult( requestCode, resultCode, data) | 请在您的Activity中重写onActivityResult方法并将相关参数传入本工具的此方法中 | 必须
setReturnPhoto(ReturnPhoto) | 回调监听器 | 可选

需要的权限：
```
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

### 准备
1) 修改 AndroidManifest.xml，添加上述权限。

然后初始化 TakePhoto：
```
TakePhotoUtil.getInstance(您的Activity).setReturnPhoto(new TakePhotoUtil.ReturnPhoto() {
            @Override
            public void onGetPhoto(String path, Bitmap bitmap) {

            }

            @Override
            public void onError(Exception e) {
                e.printStackTrace();
            }
        });
```
请注意，在您第一次调用 getInstance() 方法时会触发权限申请。

此回调方法中，path 为返回的文件路径，bitmap 为已处理好的位图数据。若产生错误，会在 onError 中返回。

2) 请在您的 Activity 中重写 onActivityResult 方法，并将它的数据传入 TakePhotoUtil：

```
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    TakePhotoUtil.getInstance(MainActivity.this).onActivityResult(requestCode, resultCode, data);
}
```

3) 调用相应方法使用相机及相册：
```
//使用相机拍摄
TakePhotoUtil.getInstance(MainActivity.this).doOpenCamera();
//使用相册选择
TakePhotoUtil.getInstance(MainActivity.this).doOpenGallery();
```

### 其他
调整图片压缩选项：

```
//初始化
TakePhotoUtil.DEFAULT_QUALITY = 90;                                 //压缩框架：图片质量
TakePhotoUtil.DEFAULT_MAX_WIDTH = 1080;                             //压缩框架：图片最大宽度
TakePhotoUtil.DEFAULT_MAX_HEIGHT = 1080;                            //压缩框架：图片最大高度
TakePhotoUtil.DEFAULT_PIC_TYPE = Bitmap.CompressFormat.JPEG;        //压缩框架：默认压缩格式
```

## 开源协议
```
   Copyright Kongzue KongzueTakePhoto

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

     http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
```

### 更新日志：
v2.0.1：
- 修复bug；

v2.0.0：
- 更换了图片压缩框架；
- Android Support 支持库升级到 27.1.0；

v1.0:
- 全新发布