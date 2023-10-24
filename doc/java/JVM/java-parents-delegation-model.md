## java类加载器/双亲委派

1.  类加载器
    1.  比较两个类是否"相等",只有在这两个类是由同一个类加载器加载的前提下才有意义
        1.  启动类加载器(Bootstrap ClassLoader)
            1.  负责加载放在JAVA-HOME\lib目录或者被-Xbootclasspath参数所指定的路径中存放的而且是Java虚拟机能够识别的(**按照文件名识别**,如rt.jar,名字不符合的即使放在lib目录中也不会被加载)类库
        2.  拓展类加载器(Extension ClassLoader)
            1.  负责加载放在JAVA_HOME\lib\ext目录或通过java.ext.dirs系统变量所指定的路径中所有的类库
        3.  应用程序类加载器(Application Class Loader)
            1.  由sum.misc.Launcher$AppClassLoader来实现
            2.  负责加载用户类路径(Classpath)上所有的类库
2.  双亲委派
    1.  如果一个类加载器收到了类加载的请求**,不会自己去尝试加载这个类,而是将请求委派给父类加载器去完成**,每一个层次的类加载器都是如此,因此所有的加载**请求最终都应该传送到最顶层的启动类加载器**中,只有当父加载器反馈自己无法完成这个加载请求(它的搜索范围中没有找到所需的类),子加载器才会尝试自己去加载
    2.  **在JDK9中,类加载的委派关系发生了变动**
        1.  当平台及应用程序类加载器收到类加载请求,在委派给父加载器加载前,要先**判断该类是否能够归属到某一个系统模块**中,如果能找到这样的归属关系,**优先委派给负责那个模块的加载器完成加载**
    3.  OSGI