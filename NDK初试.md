#NDK开发，没有你想象的那么难
*本文面向的是有一定安卓基础，但未接触过NDK的读者。*
<p>
    NDK：Native Development Kit原生开发工具<br/>
</p>
<p>
    NDK能干什么：NDK使得在android中，java可以调用C函数库。
</p>
<p>
    为什么要用NDK：我们都知道，java是半解释型语言，很容易被反汇编后拿到源代码文件，在开发一些重要协议时，我们为了安全起见，使用C语言来编写这些重要的部分，来增大系统的安全性。还有，在一些接近硬件环境下，相信大家都清楚C与java的优劣。顺带提一下：<span style="color: rgb(255, 0, 0);">NDK并不能显著提升应用效率</span>。why？我们都觉得C语言比起java来说效率要高出很多，一方面，随着jdk的不断更新，java的效率也随之提高；另一方面，即便使用C语言编码提高了应用效率，但是在java与C相互调用时平白又增大了开销。
</p>
<p>
    对于这些问题，这里就不多说了，希望详细了解的，请各位自行搜索。
</p>
<p>
    NDK开发，第一步，当然是搭建环境
</p>
<p>
    首先，去 <a href="http://developer.android.com/tools/sdk/ndk/index.html" rel="nofollow">http://developer.android.com/tools/sdk/ndk/index.html</a> 下载你对应平台的开发工具
</p>
<p>
    接着，我们需要实现linux环境 下载cygwin&nbsp; <a href="http://www.cygwin.com/" rel="nofollow">http://www.cygwin.com/</a>&nbsp; （对于64位的用户，可以直接下载我已经下载好的，百度的链接应该比在线安装快一些，正在上传到我的网盘，稍后将地址放在回复里面）
</p>
<p>
    选择在线下载的朋友，建议选择下图的地址，（是国内的）
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2014/0330/203712_R4BP_863548.png"/>
</p>
<p>
    选择好下载源以后就是选择下载目录了。我们用鼠标点开组件列表中的“Devel”分支，在该分支下，有很多组件， <br/>
</p>
<p>
    我们必须的是：binutils，gcc，gcc-mingw，gdb
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2014/0330/204115_MkAq_863548.png"/>
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2014/0330/204116_9Vc0_863548.png"/>
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2014/0330/204117_fuWV_863548.png"/>
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2014/0330/204118_kvQC_863548.png"/>
</p>
<p>
    选好这四个目录了以后，就是漫长的等待了，可以去吃顿饭差不多了。
</p>
<p>
    下面该配环境变量了：NDK环境变量需要将NDK根目录（其实就是ndk-builder.cmd文件的目录）加入系统环境变量
</p>
<p>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;cygwin环境变量需要将bin目录加入系统环境变量
</p>
<p>
    例如我的路径是：C:\java\android-ndk-r7b 和 C:\java\cygwin\bin 这两个
</p>
<p>
    配置好环境后就可以开始编码了
</p>
<p>
    1、新建一个android工程
</p>
<p>
    2、在工程目录下添加名为 jni 的文件夹（必须）
</p>
<p>
    3、在jni文件夹下新建你的.c文件（我的叫Hello.c）
</p>
<p>
    4、在jni文件夹下新建名字为Android.mk文件
</p>
<p>
    .mk文件中加入
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">LOCAL_PATH := $(call my-dir)    //当前路径（如果你了解shell语言，应该可以很轻松的理解）
include $(CLEAR_VARS)
LOCAL_MODULE    := Hello        //要生成的.so库名
LOCAL_SRC_FILES := Hello.c        //你的.c文件名字
include $(BUILD_SHARED_LIBRARY)</pre>
<p></p>
<p>
    现在可以开始写我们的C代码了，当然这里不能再去从main函数开始写，而是有固定的命名方式
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2014/0330/205836_czpO_863548.png"/>
</p>
<p>
    如图，我的函数名为：Java_com_example_testndk_MainActivity_helloWorldFromC&nbsp;&nbsp;&nbsp;&nbsp; （Java_包名_类名_函数名）
</p>
<p>
    呵呵，写C的朋友可能要抱怨了，我什么时候写过这么长的函数名了。没办法，这是jni的规范，以Java_开头，后跟java应用的包名加上类名，都是以下划线分割，最后才是跟我们的C函数名
</p>
<p>
    至于参数形式以及返回值类型，我们可以去jdk目录下翻阅jni.h文件（我的jni文件目录：C:\java\jdk1.7.0_25\include\jni），有很多函数模板（不同于C++模板）
</p>
<p>
    由于源码太多大家自己去查看吧，我就不贴图了
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2014/0330/210723_ux6E_863548.png"/>
</p>
<p>
    在jni.h文件的第104行这里可以看到我们返回的jstring本质上就是一个结构体指针，从C代码里面可以看到就是一个指向字符串的指针，在java里也就是一个数组。
</p>
<p>
    好了，C代码讲解完毕，回到我们android工程。
</p>
<p>
    从刚才的C代码函数名，大家应该就可以知道我的java类名了（这是必须的，因为要一一对应嘛） <br/>需要注意的是图中红色方框中的静态代码块 <br/> <img src="http://static.oschina.net/uploads/space/2014/0330/211236_YSRI_863548.png"/> <br/> <br/> 
</p>
<p>
    学过java大家都知道，一个 类在初始化的时候最先执行的不是构造方法而是静态代码块，没错也就是这里之所以把System.loadLibrary()放到静态代码块的原因。从名字我们就可以猜到了，加载库（“Hello”）
</p>
<p>
    还记得我们在Android.mk中声明的那个Hello吗，就是那里的名字
</p>
<p>
    紧接着，看到第12行代码，回忆java知识了，用native修饰的方法，表示java的本地方法，也就是我们的C函数了。（其实这样的函数在android SDK）中并不少见，比如我们常用到的多媒体类MediaPlayer，大家可以去看看源码，这里我就不发了，里面有很多native方法，因为要调用音频驱动嘛。
</p>
<p>
    <br/> <img src="http://static.oschina.net/uploads/space/2014/0330/212230_RX5h_863548.png"/>至此，NDK工程就结束了，来测试一下吧。首先编译我们的C代码。打开cmd，切换到工程目录下（工程目录？右键工程名，properties，如上图）输入ndk-builder（当时的环境变量设置成功了吗？去看看安卓工程的libs文件夹里面是不是多出来了个libHello.so文件） <br/>然后我们再运行我们的安卓工程吧。 <br/> <br/> <br/> <br/>最后，我再说一点自己的看法吧，首先就是C语言的基础，结构体指针一定要掌握的好，好好看看jni.h文件给出了哪些函数，其中还有支持C与java交互的函数，要想用好NDK，先用好JNI <br/> <br/> <br/>
</p>