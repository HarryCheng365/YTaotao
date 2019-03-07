## Android Studio 项目结构完全解析

1. manifests 

   AndroidManifest.xml

   项目清单文件，包含对App的一系列配置，如，应用名，所需权限，包名，所有的Activity的信息等

2. Java

   存放项目等java源代码

3. res

   - drawable

     存放不同分辨率的图片资源

   - layout

     存放项目的布局文件

   - menu

     也用于存放项目的布局文件，不过一般只存放menu的布局文件

   - mipmap

     用于存放不同分辨率的图片资源，不过在图片缩放的优化和性能上，mipmap比drawable要好

   - raw

     可选，一般用于存放数据库相关的资源

   - transition

     可选，一般用于存放slide文件

   - values

     colors.xml

     用于存放项目会用到的颜色数据

     dimens.xml

     用于存放项目会用的长度和大小等数据

     strings

     用于存放项目中会使用到的字符串数据，可根据系统语言不通，适配不同的strings.xml

4. Gradle Scripts

   - build.gradle(Project:[project name])

     包含当前项目正在使用的gradle版本号信息

   - build.gradle（Module:app)

     包含当前项目的applicationId，最小适配的Android版本，目标适配的Android版本，编译序号，应用版本号，所有依赖的包等信息

   - gradle-warpper.properties

   - proguard-rules.pro

   - setting.gradle

   - local.properties

     包含当前Android Studio编译器所使用的外部SDK路径信息



interface - based

list is null || element is null  一者为空 则 throw exception



6.3的习题 

6.4的习题 1和2  2比较难 选做

