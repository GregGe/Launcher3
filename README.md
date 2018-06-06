# Launcher3

​        本工程旨在创建一个可以在Android Studio中编译、调试的Launcher3工程。如果有其他Android App有同样的需求，可做参考。 

 	由于Launcher3使用了一些私有的API,比如`android.app.WallpaperColors`,这些导致Android Studio使用标准的sdk编译不过。

## 1. 获取framework.jar
`framework.jar`可通过编译android官方的aosp工程获得，编译时间较长。生成的路径为：

```java
out/target/common/obj/JAVA_LIBRARIES/framework_intermediates/classes.jar
```

如果不知道如何编译android源码，可参考[Google官方构建编译环境指南](https://source.android.com/source/initializing)，当然如果不敢兴趣，可使用我上传的`freeme-framework.jar`.

## 2. 放置framework.jar
新建`libs`目录，在本文我重命名为 `freeme-framework.jar`并放在此`libs`目录。

## 3. 修改build.gradle

```groovy
dependencies {
    //provided是指这个库仅仅在编译时使用，但最终不会被编译到apk或aar里
    provided files('libs/freeme-framework.jar')
    
    ...
}

//添加此处配置,意在调整编译时优先加载freeme-framework.jar，这样才能让使用某个公共类中的私有api不会报错
//-Xbootclasspath/p是java编址的优先寻址设置
//"Xbootclasspath/p"后接的路径，是相对于当前Project的根目录的相对路径
gradle.projectsEvaluated {
    tasks.withType(JavaCompile) {
        options.compilerArgs << '-Xbootclasspath/p:libs/freeme-framework.jar'
    }
}
```

## 最后，就可以顺利运行了

