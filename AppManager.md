#androidӦ�ÿ�ܴ------AppManager
*�����ܽ��һЩ��׿Ӧ�ÿ����ľ���*
<p>
    .�����ǿ���Ӧ�õ�ʱ�򣬾������кܶ�ܶ��activity����ʱ�����Ǿ���Ҫһ��activityջ����æ����activity��finish��start��
</p>
<p>
    ����OSC�İ�׿�ͻ���Ϊ��������ʹ����һ��stack&lt;Activity&gt;������ȫ����activity��
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">/**
 * Ӧ�ó���Activity�����ࣺ����Activity�����Ӧ�ó����˳�
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
     * ��ʵ�� , UI���迼�Ƕ��߳�ͬ������
     */
    public static AppManager getAppManager() {
        if (instance == null) {
            instance = new AppManager();
        }
        return instance;
    }

    /**
     * ���Activity��ջ
     */
    public void addActivity(BaseActivity activity) {
        if (activityStack == null) {
            activityStack = new Stack&lt;BaseActivity&gt;();
        }
        activityStack.add(activity);
    }

    /**
     * ��ȡ��ǰActivity��ջ��Activity��
     */
    public BaseActivity currentActivity() {
        if (activityStack == null || activityStack.isEmpty()) {
            return null;
        }
        BaseActivity activity = activityStack.lastElement();
        return activity;
    }

    /**
     * ��ȡ��ǰActivity��ջ��Activity�� û���ҵ��򷵻�null
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
     * ������ǰActivity��ջ��Activity��
     */
    public void finishActivity() {
        BaseActivity activity = activityStack.lastElement();
        finishActivity(activity);
    }

    /**
     * ����ָ����Activity(����)
     */
    public void finishActivity(Activity activity) {
        if (activity != null) {
            activityStack.remove(activity);
            activity.finish();
            activity = null;
        }
    }

    /**
     * ����ָ����Activity(����)
     */
    public void finishActivity(Class&lt;?&gt; cls) {
        for (BaseActivity activity : activityStack) {
            if (activity.getClass().equals(cls)) {
                finishActivity(activity);
            }
        }
    }

    /**
     * �رճ���ָ��activity�����ȫ��activity ���cls��������ջ�У���ջȫ�����
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
     * ��������Activity
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
     * Ӧ�ó����˳�
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
    �����Ƕ�����Ӧ�õ�activity���������Կ��������˳�Ӧ�õķ������ر�ָ��activity�ķ������ر�ȫ��activity�ķ������Լ��رճ���ָ��activity�����ȫ��activity��
</p>
<p>
    ��ô˵һ�����������ðɣ����ȣ�����ʹ��һ������ģʽȥ����ʹ������Ӧ�����κεط������Է������activityջ�������ͷ�����Ӧ�õĲ�����
</p>
<p>
    �������ǿ�����������һ��Toast
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">public static showMessage(String msg){
    Toast.makeText(AppManager.getAppManager().currentActivity(), msg, Toast.LENGTH_SHORT).show();
}</pre>
<p></p>
<p>
    ���Կ��������Ƕ�����һ��������ȫ��ʹ�õ�Toast��������Context�����ƣ���Ȼ��ʹ��֮ǰ����Ҫ����ȷ�����Ӧ��û�б�ϵͳ���١�<br/>
</p>
<p>
    �ٱ���������ʱ����һ��service����ҵ����Ȼ���뷵�ش�������ʱ��ȴ��֪����ʱ��activity�Ƿ����ɴ��ڣ����п����Ѿ����û��رգ�����ʱ�Ϳ���ʹ��activityջ��ȡ����ǰջ����activityͨ��instanceof�ؼ����ж��Ƿ���������Ҫ��activity��
</p>
<p>
    ������÷������ȥ����ɡ�<br/>
</p>
<p>
    <br/>
</p>