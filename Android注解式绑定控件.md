#Android注解式绑定控件
<p>
    Android开发中，有一个让人又爱又恨的方法叫findViewById(int);我想如果你是一民Android开发者，必然知道这个方法。</span>
</p>
<p>
    为什么说<span style="line-height: 22.5px;">findViewById(int);</span>让人又爱又恨呢？想必大家也是很有感触。<br/>写一个布局，用Java代码写和用xml文件写，完成速度完全是无法比拟的。xml布局太方便了。<br/>同样的，想获取一个控件的对象，如果你是使用的xml布局文件写的布局，那么你必须调用findViewById()这个方法。<br/>
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">TextView t = (TextView) findViewById(R.id.x);</pre>
<p></p>
<p>
    这是我们最常见的 获取xml布局中一个textview对象的过程。<br/>那么问题就来了，这特么奇葩的方法名也太长了吧！！！好吧，其实人家名字起的也没有错，要描述清楚这函数的含义，也必须这么多个字母。<br/>可是你丫的返回一个View让我用的时候还得强转，这也太麻烦了吧。我一行代码总共也就100列（Eclipse默认），缩进八格（方法写在类里面，语句写在方法里面），<br/>就算像上面的例子textView对象只有一个字母，id也只有一个字母，这一个初始化也要占我54列了。要是变量名再长点，缩进层次再深点，这一个初始化就两行了。<br/>一个界面至少也有四个控件吧，这么复杂的初始化，太坑爹了。<br/>有问题总会有对应的解决办法，下面我就向大家介绍一下使用注解解决这种麻烦。
</p>
<h3>
    了解注解：<br/>
</h3>
<p>
    从jdk1.5开始，Java提供了注解的功能，允许开发者定义和使用自己的注解类型，该功能由一个定义注解类型的语法和描述一个注解声明的语法，读取注解的API，一个使用注解修饰的class文件和一个注解处理工具组成。<br/>首先，你需要接受一个关键字<a href="http://my.oschina.net/u/996807" target="_blank" rel="nofollow">@interface</a> ,噢，它可不是接口定义关键字，更不是OC里面的@interface关键字，是Java中表示声明一个注解类的关键字。<br/>使用<span style="background-color: rgb(255, 0, 0);"></span><span style="color: rgb(255, 0, 0);"><a href="http://my.oschina.net/u/996807" target="_blank" rel="nofollow">@interface</a> </span>表示我们已经继承了java.lang.annotation.Annotation类，这是一个注解的基类接口，就好像Object类，现在你只需要知道它的存在就行了。<br/>还有一条规定：在定义注解时，不能继承其他的注解或接口。<br/>那么，这就是最简单的一个注解类<br/>
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">public @interface MyAnnotation {

}</pre>
<p></p>
<p>
    然而通常在使用时我们都会给这个注解类加上两个注解：
</p>
<p>
    @Target(ElementType.FIELD)、@Retention(RetentionPolicy.RUNTIME)<br/>ElementType、RetentionPolicy是两个枚举类，ElementType.FIELD表示我们需要注解的是一个字段，以下是摘自JDK1.6文档中的介绍：
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2014/0823/153748_usr8_863548.png"/>
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2014/0823/153748_JBec_863548.png"/>
</p>
<h3>
    使用注解：
</h3>
<p>
    以下为KJFrameForAndroid框架中绑定控件注解部分的定义与使用：
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
public @interface BindView {
    public int id();
    public boolean click() default false;
}</pre>
<p></p>
<pre class="brush:java;toolbar: true; auto-links: false;">@BindView(id = R.id.x, click = true)
private TextView t;</pre>
<p></p>
<p>
    我们可以看到，除了明显减少了代码量，还使得代码结构更加清晰。<br/>其中，定义部分的id() 表示注解接受一个int类型的数据作为id所对应的值（就如使用中的id = R.id.xxx）;<br/>同理，定义部分的click表示接受一个Boolean类型的数据作为click对应的值，还可以设置一个默认值使用default修饰；
</p>
<h3>
    处理注解：
</h3>
<p>
    我们已经知道了注解怎么定义和使用，接下来就应该知道怎么处理了。<br/>上面已经说了，bindview注解可以接受一个int类型的值和一个Boolean类型的值，那么这两个值接受了以后如何获取呢？<br/>其实获取的方式很简单就是通过一个BindView类型的对象，调用这个对象来自声明中定义的两个方法——&gt;id()或click()方法。<br/>现在就有一个问题了，注解类型是不能直接new对象的，那么这个BindView对象从哪里来呢？<br/>这时就需要用到Java的反射机制。我们知道，每一个继承自Object类的类都会继承一个getClass()方法，下面看一下这个方法的原型：
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">    /**
     * Returns the unique instance of {@link Class} that represents this
     * object&#39;s class. Note that {@code getClass()} is a special case in that it
     * actually returns {@code Class&lt;? extends Foo&gt;} where {@code Foo} is the
     * erasure of the type of the expression {@code getClass()} was called upon.
     * &lt;p&gt;
     * As an example, the following code actually compiles, although one might
     * think it shouldn&#39;t:
     * &lt;p&gt;
     * &lt;pre&gt;{@code
     *   List&lt;Integer&gt; l = new ArrayList&lt;Integer&gt;();
     *   Class&lt;? extends List&gt; c = l.getClass();}&lt;/pre&gt;
     *
     * @return this object&#39;s {@code Class} instance.
     */
    public final native Class&lt;?&gt; getClass();</pre>
<p></p>
<p>
    是一个native方法，根据注释我们知道，这个方法返回的是该类的Class对象，同时也是该类的二进制对象。<br/>Class中有一个方法叫getDeclaredFields()，是用来返回这个类的全部字段，返回类型是Field[]<br/>通过Field对象的getAnnotation(Class&lt;?&gt;)方法，我们可以获取到任何一个Class的对象，通过getAnnotation(Class&lt;?&gt;)，我们就可以获取到BindView的对象了。
</p>
<p>
    例如：
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">Field[] fields = currentClass.getClass().getDeclaredFields();
for(int i = 0; i &lt; fields.length; i++){

    BindView bindView = field.getAnnotation(BindView.class);
    
    int viewId = bindView.id();  //这是我们传的id
    
    boolean clickLis = bindView.click(); //这是我们传的click
}</pre>
<p></p>
<h3>
    在Android项目中应用：
</h3>
<p>
    至此，我们已经了解了注解，并且知道怎么使用，怎么处理注解了，现在只剩下最后一个问题：在项目中使用。<br/>很简单，传一个Activity对象，调用findViewById()不就行了。<br/>于是，我们可以这样<br/>activity.findViewById( bindView.id() );<br/>最后在我们的Activity中调用这个函数就OK了。
</p>
<p>
    以下是Android应用框架KJFrameForAndroid中使用注解绑定控件的核心代码：
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">/**
     * @param currentClass
     *            当前类，一般为Activity或Fragment
     * @param sourceView
     *            待绑定控件的直接或间接父控件
     */
    public static void initBindView(Object currentClass, View sourceView) {
        // 通过反射获取到全部属性，反射的字段可能是一个类（静态）字段或实例字段
        Field[] fields = currentClass.getClass().getDeclaredFields();
        if (fields != null &amp;&amp; fields.length &gt; 0) {
            for (Field field : fields) {
                // 返回BindView类型的注解内容
                BindView bindView = field.getAnnotation(BindView.class);
                if (bindView != null) {
                    int viewId = bindView.id();
                    boolean clickLis = bindView.click();
                    try {
                        field.setAccessible(true);
                        if (clickLis) {
                            sourceView.findViewById(viewId).setOnClickListener(
                                    (OnClickListener) currentClass);
                        }
                        // 将currentClass的field赋值为sourceView.findViewById(viewId)
                        field.set(currentClass, sourceView.findViewById(viewId));
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }</pre>
<p></p>
<p>
    其实安卓中的注解式绑定控件（也是所谓的IOC控制反转在安卓中的一种应用）其实本质的使用就是Java基础中反射的使用。值得一提的是，反射执行的效率是很低的<br/><span style="color: rgb(255, 0, 0);">如果不是必要，应当尽量减少反射的使用，因为它会大大拖累你应用的执行效率。</span><br/>顺带一提：我一直很排斥注解，因为类反射的效率太低了。现在有很多安卓应用开发框架，比如KJFrameForAndroid, xUtils, afinal, thinkAndroid，这些框架都是使用反射来起到注解绑定控件。<br/>更有的框架甚至是一切东西都使用注解去完成，我只能说注解便捷，但请慎用。<br/>
</p>