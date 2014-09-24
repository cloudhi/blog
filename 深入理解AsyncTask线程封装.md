#�������AsyncTask���̷߳�װ
*��ƪ������Ҫ������һ��Android�������ˣ�����㻹�����ţ���ƪ���¿��������ܻ�Ƚϳ�����ϣ������ѧ���¶�����*
<p>
    ��Android�����У����ڲ�����UI�߳�������ʱ������������Ҫ�����߳�����һЩ��������������һ���Ͳ�����һ�����⣬���Ǵ������̲߳���ִ�У�������߳�ά���Ŀ�������ʹ�ô��������½��ֻ������ֺĵ硣����������һ��KJFrameForAndroid�������ν���������ġ�<br/>KJFrameForAndroid�����Ŀ��ַ��<a target="_blank" href="https://github.com/kymjs/KJFrameForAndroid" rel="nofollow">https://github.com/kymjs/KJFrameForAndroid</a>��
</p>
<p>
    ��ʵAndroid�ṩ��һ��ר�������첽�������,����������Ϥ��ģʽ��AsynTask�ࡣ<br/>AsynTask����Ƕ�Thread���һ����װ�����Ҽ�����һЩ�µķ�������ô����������һ��������Thread�����һϵ�й������װ��<br/>��jdk��������һ��������У�BlockingQueue&lt;&gt;����һ�� Queue&lt;E&gt;�����֧࣬���������Ӳ����� Queue�������������ǣ���ȡԪ��ʱ�ȴ����б�Ϊ�ǿգ��Լ��洢Ԫ��ʱ�ȴ��ռ��ÿ��á� <br/>BlockingQueue ������������ʽ���֣����ڲ����������㵫�����ڽ���ĳһʱ�̿�������Ĳ�������������ʽ�Ĵ���ʽ��ͬ����һ�����׳�һ���쳣���ڶ����Ƿ���һ������ֵ��null �� false������ȡ���ڲ����������������ڲ������Գɹ�ǰ�������ڵ�������ǰ�̣߳����������ڷ���ǰֻ�ڸ��������ʱ��������������
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2014/0915/152915_Qe8X_863548.png"/><br/>
</p>
<p>
    ��������һ����jdk�й��������Ľ��ܣ�
</p>
<p>
    BlockingQueue ������ null Ԫ�ء���ͼ add��put �� offer һ�� null Ԫ��ʱ��ĳЩʵ�ֻ��׳� NullPointerException��null ������ָʾ poll ����ʧ�ܵľ���ֵ�� <br/><br/>�ã�ΪʲôҪ������������أ���Ϊ�������ǽ�����Ҫ�����̳߳�ά������Ī��Ĺ�ϵ��<br/>
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">    // ��̬����ʽ���У�������Ŵ�ִ�е����񣬳�ʼ������8��
    private static final BlockingQueue&lt;Runnable&gt; mPoolWorkQueue = new LinkedBlockingQueue&lt;Runnable&gt;(8);</pre>
<p></p>
<p>
    ������KJFrameForAndroid��������ж����̶߳��е�ά������ͨ������̶߳��н������̷߳�����һ����̬����ʽ�����б���������<br/>Ȼ���������Ǳ�����������������Ϊ�߳���Ҫִ�У����Ǳ���Ҫ���߳̽��뵽CPU��������У���ʱKJFrameForAndroidʹ����jdk�е���һ����ThreadPoolExecutor������ʹ�ò���������Щ�߳�<br/>����һ����KJFrameForAndroid�ж�����������Ĵ���<br/>
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">    /**
     * �����̳߳�����ִ������������������ִ������&lt;br&gt;
     * ��mSerialExecutor�����У����Ӧ
     */
    public static final Executor mThreadPoolExecutor = new ThreadPoolExecutor(
            CORE_POOL_SIZE, MAXIMUM_POOL_SIZE, KEEP_ALIVE,
            TimeUnit.SECONDS, mPoolWorkQueue, mThreadFactory);</pre>
<p></p>
<p>
    ͨ��ע�ͣ����ǻ����Կ���KJFrameForAndroid�����ʵ����һ�ֲ���ִ���̵߳ķ�ʽ��������ʱ���������ǿ��������Ĺ��췽����jdk�еĽ��ͣ�
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2014/0915/153043_3GhQ_863548.png"/>
</p>
<p>
    ��ô�����������˲�������ִ�������̳߳ض��У�����ֻ��Ҫ���߳�ִ�����������ˡ�ThreadPoolExecutor����һ��ʵ����ִ����Executor�ӿڵ��࣬��ô��Ҳ����һ��execute����ȥִ���̣߳�������Ҫ���ľ��ǵ����������ִ�����Ϳ����ˡ�<br/><br/>��������������һ��������߳�ִ����ɺ��ֻص�UI�߳�ȥ�����洦��ġ�<br/><br/>���ǿ�Դ�룬������ժ��KJFrameForAndroid��Դ����е�һ�δ��룺
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">/*********************** start һ��������ִ������ ***************************/
    /**
     * ��doInBackground֮ǰ���ã���������ʼ������ �����̣߳�UI�߳�
     */
    protected void onPreExecute() {}
    /**
     * ������������Ǳ���Ҫ��д�ģ���������̨���� �����̣߳���̨�߳�
     */
    protected abstract Result doInBackground(Params... params);
    /**
     * ��ӡ��̨������ȣ�onProgressUpdate�ᱻ����&lt;br&gt;
     * ʹ���ڲ�handle����һ��������Ϣ����onProgressUpdate������
     */
    protected final void publishProgress(Progress... values) {
        if (!isCancelled()) {
            mHandler.obtainMessage(MESSAGE_POST_PROGRESS,
                    new KJTaskResult&lt;Progress&gt;(this, values))
                    .sendToTarget();
        }
    }
    /**
     * ��publishProgress֮����ã��������¼������ �����̣߳�UI�߳�
     */
    protected void onProgressUpdate(Progress... values) {}
    /**
     * ���������ʱ�������жϣ��������û�б�ȡ���������onPostExecute;�������onCancelled
     */
    private void finish(Result result) {
        if (isCancelled()) {
            onCancelled(result);
        } else {
            onPostExecute(result);
        }
        mStatus = Status.FINISHED;
    }
    /**
     * ��doInBackground֮����ã��������ܺ�̨����������UI �����̣߳�UI�߳�
     */
    protected void onPostExecute(Result result) {}
    /**
     * �����̣߳�UI�߳�&lt;br&gt;
     * doInBackgroundִ�н�������{@link #cancel(boolean)} �����á�&lt;br&gt;
     * ������������������ʾ�����ѱ�ȡ�������ʱ��onPostExecute�����ٱ����á�
     */
    protected void onCancelled(Result result) {}
    /*********************** end һ��������ִ������ ***************************/</pre>
<p></p>
<p>
    ���������ǿ����������ǳ���Ϥ�ĺ�����doInBackground��onPostExecute��onProgressUpdate<br/>û��������������ʽSyncTask�б�����ʹ�õ���������������㻹��֪����������Android������SyncTask�÷������������ٿ�����������α����õģ�<br/>����������һ���߳�����ִ�еķ�����ʵ����Ҳ����doInBackground�������õķ�����
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">/**
     * ������UI�̵߳��ô˷���&lt;br&gt;
     * ͨ������������ǿ����Զ���KJTaskExecutor��ִ�з�ʽ������or���У��������Բ����Լ���Executor Ϊ��ʵ�ֲ��У�
     * asyncTask.executeOnExecutor(KJTaskExecutor.mThreadPoolExecutor, params);
     */
    public final KJTaskExecutor&lt;Params, Progress, Result&gt; executeOnExecutor(
            Executor exec, Params... params) {
        if (mStatus != Status.PENDING) {
            switch (mStatus) {
            case RUNNING:
                throw new IllegalStateException(
                        &quot;Cannot execute task:&quot;
                                + &quot; the task is already running.&quot;);
            case FINISHED:
                throw new IllegalStateException(
                        &quot;Cannot execute task:&quot;
                                + &quot; the task has already been executed &quot;
                                + &quot;(a task can be executed only once)&quot;);
            default:
                break;
            }
        }
        mStatus = Status.RUNNING;
        onPreExecute();
        mWorker.mParams = params;
        exec.execute(mFuture);// ԭ��{@link #execute(Runnable runnable)}
        // ���Ż���#onProgressUpdate�����ã������#onPostExecute
        return this;
    }</pre>
<p></p>
<p>
    ��onProgressUpdateʵ�����������handle�б������ˡ�
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">/**
     * KJTaskExecutor�ڲ�Handler���������ͺ�̨������ȸ�����Ϣ�ͼ��������Ϣ
     */
    private static class InternalHandler extends Handler {
        @Override
        @SuppressWarnings({ &quot;unchecked&quot;, &quot;rawtypes&quot; })
        public void handleMessage(Message msg) {
            KJTaskResult result = (KJTaskResult) msg.obj;
            switch (msg.what) {
            case MESSAGE_POST_RESULT:
                result.mTask.finish(result.mData[0]);
                break;
            case MESSAGE_POST_PROGRESS:
                result.mTask.onProgressUpdate(result.mData);
                break;
            }
        }
    }</pre>
<p></p>
<p>
    �йر��������������Բ鿴<a target="_blank" href="https://github.com/kymjs/KJFrameForAndroid/blob/master/KJLibrary/src/org/kymjs/aframe/core/KJTaskExecutor.java" rel="nofollow">https://github.com/kymjs/KJFrameForAndroid/blob/master/KJLibrary/src/org/kymjs/aframe/core/KJTaskExecutor.java</a><br/>
</p>