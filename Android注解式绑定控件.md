#Androidע��ʽ�󶨿ؼ�
<p>
    Android�����У���һ�������ְ��ֺ޵ķ�����findViewById(int);�����������һ��Android�����ߣ���Ȼ֪�����������</span>
</p>
<p>
    Ϊʲô˵<span style="line-height: 22.5px;">findViewById(int);</span>�����ְ��ֺ��أ���ش��Ҳ�Ǻ��ид���<br/>дһ�����֣���Java����д����xml�ļ�д������ٶ���ȫ���޷�����ġ�xml����̫�����ˡ�<br/>ͬ���ģ����ȡһ���ؼ��Ķ����������ʹ�õ�xml�����ļ�д�Ĳ��֣���ô��������findViewById()���������<br/>
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">TextView t = (TextView) findViewById(R.id.x);</pre>
<p></p>
<p>
    ������������� ��ȡxml������һ��textview����Ĺ��̡�<br/>��ô��������ˣ�����ô����ķ�����Ҳ̫���˰ɣ������ðɣ���ʵ�˼��������Ҳû�д�Ҫ��������⺯���ĺ��壬Ҳ������ô�����ĸ��<br/>������Ѿ�ķ���һ��View�����õ�ʱ�򻹵�ǿת����Ҳ̫�鷳�˰ɡ���һ�д����ܹ�Ҳ��100�У�EclipseĬ�ϣ��������˸񣨷���д�������棬���д�ڷ������棩��<br/>���������������textView����ֻ��һ����ĸ��idҲֻ��һ����ĸ����һ����ʼ��ҲҪռ��54���ˡ�Ҫ�Ǳ������ٳ��㣬�����������㣬��һ����ʼ���������ˡ�<br/>һ����������Ҳ���ĸ��ؼ��ɣ���ô���ӵĳ�ʼ����̫�ӵ��ˡ�<br/>�������ܻ��ж�Ӧ�Ľ���취�������Ҿ����ҽ���һ��ʹ��ע���������鷳��
</p>
<h3>
    �˽�ע�⣺<br/>
</h3>
<p>
    ��jdk1.5��ʼ��Java�ṩ��ע��Ĺ��ܣ��������߶����ʹ���Լ���ע�����ͣ��ù�����һ������ע�����͵��﷨������һ��ע���������﷨����ȡע���API��һ��ʹ��ע�����ε�class�ļ���һ��ע�⴦������ɡ�<br/>���ȣ�����Ҫ����һ���ؼ���<a href="http://my.oschina.net/u/996807" target="_blank" rel="nofollow">@interface</a> ,�ޣ����ɲ��ǽӿڶ���ؼ��֣�������OC�����@interface�ؼ��֣���Java�б�ʾ����һ��ע����Ĺؼ��֡�<br/>ʹ��<span style="background-color: rgb(255, 0, 0);"></span><span style="color: rgb(255, 0, 0);"><a href="http://my.oschina.net/u/996807" target="_blank" rel="nofollow">@interface</a> </span>��ʾ�����Ѿ��̳���java.lang.annotation.Annotation�࣬����һ��ע��Ļ���ӿڣ��ͺ���Object�࣬������ֻ��Ҫ֪�����Ĵ��ھ����ˡ�<br/>����һ���涨���ڶ���ע��ʱ�����ܼ̳�������ע���ӿڡ�<br/>��ô���������򵥵�һ��ע����<br/>
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">public @interface MyAnnotation {

}</pre>
<p></p>
<p>
    Ȼ��ͨ����ʹ��ʱ���Ƕ�������ע�����������ע�⣺
</p>
<p>
    @Target(ElementType.FIELD)��@Retention(RetentionPolicy.RUNTIME)<br/>ElementType��RetentionPolicy������ö���࣬ElementType.FIELD��ʾ������Ҫע�����һ���ֶΣ�������ժ��JDK1.6�ĵ��еĽ��ܣ�
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2014/0823/153748_usr8_863548.png"/>
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2014/0823/153748_JBec_863548.png"/>
</p>
<h3>
    ʹ��ע�⣺
</h3>
<p>
    ����ΪKJFrameForAndroid����а󶨿ؼ�ע�ⲿ�ֵĶ�����ʹ�ã�
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
    ���ǿ��Կ������������Լ����˴���������ʹ�ô���ṹ����������<br/>���У����岿�ֵ�id() ��ʾע�����һ��int���͵�������Ϊid����Ӧ��ֵ������ʹ���е�id = R.id.xxx��;<br/>ͬ�����岿�ֵ�click��ʾ����һ��Boolean���͵�������Ϊclick��Ӧ��ֵ������������һ��Ĭ��ֵʹ��default���Σ�
</p>
<h3>
    ����ע�⣺
</h3>
<p>
    �����Ѿ�֪����ע����ô�����ʹ�ã���������Ӧ��֪����ô�����ˡ�<br/>�����Ѿ�˵�ˣ�bindviewע����Խ���һ��int���͵�ֵ��һ��Boolean���͵�ֵ����ô������ֵ�������Ժ���λ�ȡ�أ�<br/>��ʵ��ȡ�ķ�ʽ�ܼ򵥾���ͨ��һ��BindView���͵Ķ��󣬵�������������������ж����������������&gt;id()��click()������<br/>���ھ���һ�������ˣ�ע�������ǲ���ֱ��new����ģ���ô���BindView������������أ�<br/>��ʱ����Ҫ�õ�Java�ķ�����ơ�����֪����ÿһ���̳���Object����඼��̳�һ��getClass()���������濴һ�����������ԭ�ͣ�
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
    ��һ��native����������ע������֪��������������ص��Ǹ����Class����ͬʱҲ�Ǹ���Ķ����ƶ���<br/>Class����һ��������getDeclaredFields()������������������ȫ���ֶΣ�����������Field[]<br/>ͨ��Field�����getAnnotation(Class&lt;?&gt;)���������ǿ��Ի�ȡ���κ�һ��Class�Ķ���ͨ��getAnnotation(Class&lt;?&gt;)�����ǾͿ��Ի�ȡ��BindView�Ķ����ˡ�
</p>
<p>
    ���磺
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">Field[] fields = currentClass.getClass().getDeclaredFields();
for(int i = 0; i &lt; fields.length; i++){

    BindView bindView = field.getAnnotation(BindView.class);
    
    int viewId = bindView.id();  //�������Ǵ���id
    
    boolean clickLis = bindView.click(); //�������Ǵ���click
}</pre>
<p></p>
<h3>
    ��Android��Ŀ��Ӧ�ã�
</h3>
<p>
    ���ˣ������Ѿ��˽���ע�⣬����֪����ôʹ�ã���ô����ע���ˣ�����ֻʣ�����һ�����⣺����Ŀ��ʹ�á�<br/>�ܼ򵥣���һ��Activity���󣬵���findViewById()�������ˡ�<br/>���ǣ����ǿ�������<br/>activity.findViewById( bindView.id() );<br/>��������ǵ�Activity�е������������OK�ˡ�
</p>
<p>
    ������AndroidӦ�ÿ��KJFrameForAndroid��ʹ��ע��󶨿ؼ��ĺ��Ĵ��룺
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">/**
     * @param currentClass
     *            ��ǰ�࣬һ��ΪActivity��Fragment
     * @param sourceView
     *            ���󶨿ؼ���ֱ�ӻ��Ӹ��ؼ�
     */
    public static void initBindView(Object currentClass, View sourceView) {
        // ͨ�������ȡ��ȫ�����ԣ�������ֶο�����һ���ࣨ��̬���ֶλ�ʵ���ֶ�
        Field[] fields = currentClass.getClass().getDeclaredFields();
        if (fields != null &amp;&amp; fields.length &gt; 0) {
            for (Field field : fields) {
                // ����BindView���͵�ע������
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
                        // ��currentClass��field��ֵΪsourceView.findViewById(viewId)
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
    ��ʵ��׿�е�ע��ʽ�󶨿ؼ���Ҳ����ν��IOC���Ʒ�ת�ڰ�׿�е�һ��Ӧ�ã���ʵ���ʵ�ʹ�þ���Java�����з����ʹ�á�ֵ��һ����ǣ�����ִ�е�Ч���Ǻܵ͵�<br/><span style="color: rgb(255, 0, 0);">������Ǳ�Ҫ��Ӧ���������ٷ����ʹ�ã���Ϊ������������Ӧ�õ�ִ��Ч�ʡ�</span><br/>˳��һ�᣺��һֱ���ų�ע�⣬��Ϊ�෴���Ч��̫���ˡ������кܶల׿Ӧ�ÿ�����ܣ�����KJFrameForAndroid, xUtils, afinal, thinkAndroid����Щ��ܶ���ʹ�÷�������ע��󶨿ؼ���<br/>���еĿ��������һ�ж�����ʹ��ע��ȥ��ɣ���ֻ��˵ע���ݣ��������á�<br/>
</p>