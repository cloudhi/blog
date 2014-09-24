#android应用框架搭建------AppManager
*个人总结的一些安卓应用开发的经验*
<p>
    .在我们开发应用的时候，经常会有很多很多的activity，这时候，我们就需要一个activity栈来帮忙管理activity的finish和start。
</p>
<p>
    就拿OSC的安卓客户端为例，代码使用了一个stack&lt;Activity&gt;来保存全部的activity。
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">/**
 * 应用程序Activity管理类：用于Activity管理和应用程序退出
 * 
 * @author kymjs
 * @version 1.0
 * @created 2013-11-24
 */
public class AppManager {
    private static Stack&lt;BaseActivity&gt; activityStack;
    private static AppManager instance;

    private AppManager() {
    }

    /**
     * 单实例 , UI无需考虑多线程同步问题
     */
    public static AppManager getAppManager() {
        if (instance == null) {
            instance = new AppManager();
        }
        return instance;
    }

    /**
     * 添加Activity到栈
     */
    public void addActivity(BaseActivity activity) {
        if (activityStack == null) {
            activityStack = new Stack&lt;BaseActivity&gt;();
        }
        activityStack.add(activity);
    }

    /**
     * 获取当前Activity（栈顶Activity）
     */
    public BaseActivity currentActivity() {
        if (activityStack == null || activityStack.isEmpty()) {
            return null;
        }
        BaseActivity activity = activityStack.lastElement();
        return activity;
    }

    /**
     * 获取当前Activity（栈顶Activity） 没有找到则返回null
     */
    public BaseActivity findActivity(Class&lt;?&gt; cls) {
        BaseActivity activity = null;
        for (BaseActivity aty : activityStack) {
            if (aty.getClass().equals(cls)) {
                activity = aty;
                break;
            }
        }
        return activity;
    }

    /**
     * 结束当前Activity（栈顶Activity）
     */
    public void finishActivity() {
        BaseActivity activity = activityStack.lastElement();
        finishActivity(activity);
    }

    /**
     * 结束指定的Activity(重载)
     */
    public void finishActivity(Activity activity) {
        if (activity != null) {
            activityStack.remove(activity);
            activity.finish();
            activity = null;
        }
    }

    /**
     * 结束指定的Activity(重载)
     */
    public void finishActivity(Class&lt;?&gt; cls) {
        for (BaseActivity activity : activityStack) {
            if (activity.getClass().equals(cls)) {
                finishActivity(activity);
            }
        }
    }

    /**
     * 关闭除了指定activity以外的全部activity 如果cls不存在于栈中，则栈全部清空
     * 
     * @param cls
     */
    public void finishOthersActivity(Class&lt;?&gt; cls) {
        for (BaseActivity activity : activityStack) {
            if (!(activity.getClass().equals(cls))) {
                finishActivity(activity);
            }
        }
    }

    /**
     * 结束所有Activity
     */
    public void finishAllActivity() {
        for (int i = 0, size = activityStack.size(); i &lt; size; i++) {
            if (null != activityStack.get(i)) {
                activityStack.get(i).finish();
            }
        }
        activityStack.clear();
    }

    /**
     * 应用程序退出
     */
    public void AppExit(Context context) {
        try {
            finishAllActivity();
            ActivityManager activityMgr = (ActivityManager) context
                    .getSystemService(Context.ACTIVITY_SERVICE);
            activityMgr.killBackgroundProcesses(context.getPackageName());
            System.exit(0);
        } catch (Exception e) {
            System.exit(0);
        }
    }
}</pre>
<p></p>
<p>
    这里是对整个应用的activity操作，可以看到，有退出应用的方法，关闭指定activity的方法，关闭全部activity的方法，以及关闭除了指定activity以外的全部activity。
</p>
<p>
    那么说一下这个类的作用吧，首先，该类使用一个单例模式去管理，使得整个应用在任何地方都可以访问这个activity栈，这样就方便了应用的操作。
</p>
<p>
    例如我们可以这样定义一个Toast
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">public static showMessage(String msg){
    Toast.makeText(AppManager.getAppManager().currentActivity(), msg, Toast.LENGTH_SHORT).show();
}</pre>
<p></p>
<p>
    可以看到，我们定义了一个可以在全局使用的Toast，不再受Context的限制，当然在使用之前你需要首先确定你的应用没有被系统销毁。<br/>
</p>
<p>
    再比如我们有时候在一个service中做业务处理，然后想返回处理结果的时候，却不知道当时的activity是否依旧存在（它有可能已经被用户关闭），此时就可以使用activity栈获取到当前栈顶的activity通过instanceof关键字判断是否是我们想要的activity。
</p>
<p>
    更多的用法，大家去发掘吧。<br/>
</p>
<p>
    <br/>
</p>