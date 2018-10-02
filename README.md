# testMyPlugin
create my first plugin demo, just to test.
## 步骤
### 1.(对于供其他工程使用的插件)创建module， 
### 2.删除module中的无用文件，保留build.gradle 和 main目录
### 3.添加目录，创建实现Plugin的类，然后重写apply()方法，实现你的任务
### 4.在main目录下添加resources目录，在resources目录下添加META-INF目录，在META-INF目录下添加gradle-plugins目录，在gradle-plugins目录下创建新的properties文件，这个properties文件的名字，你可以随意取名，但是后面使用这个插件的时候，会用到这个名字。
### 5.在properties文件中声明实现的插件的类 implementation-class=com.wuzp.plugin.MyPlugin
### 6.因为我们要用到groovy以及后面打包要用到maven,所以在我们自定义的Module下的build.gradle需要添加如下代码：
```java
apply plugin: 'groovy'
apply plugin: 'maven'

dependencies {
    //gradle sdk
    compile gradleApi()
    //groovy sdk
    compile localGroovy()
}

repositories {
    mavenCentral()
}
```
### 7.对于打包到本地的的设置，在module的build文件中添加如下配置
```groovy
//group和version在后面使用自定义插件的时候会用到
group='com.wuzp.plugin'
version='1.0.0'

uploadArchives {
    repositories {
        mavenDeployer {
            //提交到远程服务器：
           // repository(url: "http://www.xxx.com/repos") {
            //    authentication(userName: "admin", password: "admin")
           // }
           //本地的Maven地址设置为/Users/didi/SelfWork/testDemo/testMyPlugin/MyRepos
            repository(url: uri('/Users/didi/SelfWork/testDemo/testMyPlugin/MyRepos'))
        }
    }
}

```
### 8.点击module下的编译任务upload，然后编译成功之后几句能在目录下看到生成的插件文件
### 9.步骤8上边生成的maven库在本地，所以引用也只能在本地引用。如果公司有服务器的法，可以将插件传到服务器上 供大家使用
### 10.对于其他的工程想引用这个插件需要做的是
#### 1).在根目录下的build文件中 添加maven库路径
```groovy
buildscript {
    repositories {
        maven {//本地Maven仓库地址
            url uri('/Users/didi/SelfWork/testDemo/testMyPlugin/MyRepos')
        }
    }
    dependencies {
        //格式为-->group:module:version
        classpath 'com.wuzp.plugin:mypluginlib:1.0.0'
    }
}

```
#### 2).在需要引入的插件的build文件中引入插件
```groovy
apply plugin: 'com.wuzp.gradle'
```
#### 3).编译就可以了


##### 参考：https://blog.csdn.net/huachao1001/article/details/51810328
