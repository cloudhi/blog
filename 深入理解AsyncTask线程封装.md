#深入理解AsyncTask类线程封装
*本篇文章主要面向有一定Android基础的人，如果你还刚入门，这篇文章看起来可能会比较吃力，希望你能学到新东西。*
<p>
    在Android开发中，由于不能再UI线程中做耗时操作，常常需要开启线程来做一些操作。但是这样一来就产生了一个问题，就是大量的线程并发执行，造成了线程维护的开销进而使得代码质量下降手机发烫又耗电。让我们来看一下KJFrameForAndroid框架是如何解决这个问题的。<br/>KJFrameForAndroid框架项目地址：<a target="_blank" href="https://github.com/kymjs/KJFrameForAndroid" rel="nofollow">https://github.com/kymjs/KJFrameForAndroid</a>。
</p>
<p>
    其实Android提供了一套专门用于异步处理的类,就是我们熟悉又模式的AsynTask类。<br/>AsynTask类就是对Thread类的一个封装，并且加入了一些新的方法。那么我们先来看一下它对于Thread请求的一系列管理与封装。<br/>在jdk中有这样一个集合类叫：BlockingQueue&lt;&gt;它是一个 Queue&lt;E&gt;的子类，支持两个附加操作的 Queue，这两个操作是：获取元素时等待队列变为非空，以及存储元素时等待空间变得可用。 <br/>BlockingQueue 方法以四种形式出现，对于不能立即满足但可能在将来某一时刻可以满足的操作，这四种形式的处理方式不同：第一种是抛出一个异常，第二种是返回一个特殊值（null 或 false，具体取决于操作），第三种是在操作可以成功前，无限期地阻塞当前线程，第四种是在放弃前只在给定的最大时间限制内阻塞。
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2014/0915/152915_Qe8X_863548.png"/><br/>
</p>
<p>
    我们来看一下在jdk中关于这个类的介绍：
</p>
<p>
    BlockingQueue 不接受 null 元素。试图 add、put 或 offer 一个 null 元素时，某些实现会抛出 NullPointerException。null 被用作指示 poll 操作失败的警戒值。 <br/><br/>好，为什么要讲这个集合类呢？因为这与我们接下来要讲的线程池维护有着莫大的关系。<br/>
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">    // 静态阻塞式队列，用来存放待执行的任务，初始容量：8个
    private static final BlockingQueue&lt;Runnable&gt; mPoolWorkQueue = new LinkedBlockingQueue&lt;Runnable&gt;(8);</pre>
<p></p>
<p>
    这里是KJFrameForAndroid开发框架中对于线程队列的维护对象，通过这个线程队列将所有线程放置在一个静态阻塞式队列中保存起来。<br/>然而，仅仅是保存起来还不够，因为线程需要执行，我们必须要让线程进入到CPU工作间才行，这时KJFrameForAndroid使用了jdk中的另一个类ThreadPoolExecutor来管理并使用并发启动这些线程<br/>来看一下在KJFrameForAndroid中对于这个类对象的创建<br/>
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">    /**
     * 并发线程池任务执行器，可以用来并行执行任务&lt;br&gt;
     * 与mSerialExecutor（串行）相对应
     */
    public static final Executor mThreadPoolExecutor = new ThreadPoolExecutor(
            CORE_POOL_SIZE, MAXIMUM_POOL_SIZE, KEEP_ALIVE,
            TimeUnit.SECONDS, mPoolWorkQueue, mThreadFactory);</pre>
<p></p>
<p>
    通过注释，我们还可以看到KJFrameForAndroid框架其实还有一种并行执行线程的方式，这里暂时不讲，我们看看这个类的构造方法在jdk中的解释：
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2014/0915/153043_3GhQ_863548.png"/>
</p>
<p>
    那么至此我们有了并发任务执行器和线程池队列，最后就只需要将线程执行起来就行了。ThreadPoolExecutor类是一个实现了执行器Executor接口的类，那么它也就有一个execute方法去执行线程，我们所要做的就是调用这个类来执行它就可以了。<br/><br/>接下来我们来看一下如何让线程执行完成后又回到UI线程去做界面处理的。<br/><br/>还是看源码，以下是摘自KJFrameForAndroid开源框架中的一段代码：
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">/*********************** start 一个完整的执行周期 ***************************/
    /**
     * 在doInBackground之前调用，用来做初始化工作 所在线程：UI线程
     */
    protected void onPreExecute() {}
    /**
     * 这个方法是我们必须要重写的，用来做后台计算 所在线程：后台线程
     */
    protected abstract Result doInBackground(Params... params);
    /**
     * 打印后台计算进度，onProgressUpdate会被调用&lt;br&gt;
     * 使用内部handle发送一个进度消息，让onProgressUpdate被调用
     */
    protected final void publishProgress(Progress... values) {
        if (!isCancelled()) {
            mHandler.obtainMessage(MESSAGE_POST_PROGRESS,
                    new KJTaskResult&lt;Progress&gt;(this, values))
                    .sendToTarget();
        }
    }
    /**
     * 在publishProgress之后调用，用来更新计算进度 所在线程：UI线程
     */
    protected void onProgressUpdate(Progress... values) {}
    /**
     * 任务结束的时候会进行判断：如果任务没有被取消，则调用onPostExecute;否则调用onCancelled
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
     * 在doInBackground之后调用，用来接受后台计算结果更新UI 所在线程：UI线程
     */
    protected void onPostExecute(Result result) {}
    /**
     * 所在线程：UI线程&lt;br&gt;
     * doInBackground执行结束并且{@link #cancel(boolean)} 被调用。&lt;br&gt;
     * 如果本函数被调用则表示任务已被取消，这个时候onPostExecute不会再被调用。
     */
    protected void onCancelled(Result result) {}
    /*********************** end 一个完整的执行周期 ***************************/</pre>
<p></p>
<p>
    在这里我们看到了三个非常熟悉的函数：doInBackground、onPostExecute、onProgressUpdate<br/>没错，这三个函数正式SyncTask中被我们使用的三个方法（如果你还不知道，请搜索Android开发中SyncTask用法）接着我们再看看他们是如何被调用的：<br/>这里声明了一个线程任务被执行的方法，实际上也就是doInBackground、被调用的方法：
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">/**
     * 必须在UI线程调用此方法&lt;br&gt;
     * 通过这个方法我们可以自定义KJTaskExecutor的执行方式，串行or并行，甚至可以采用自己的Executor 为了实现并行，
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
        exec.execute(mFuture);// 原理{@link #execute(Runnable runnable)}
        // 接着会有#onProgressUpdate被调用，最后是#onPostExecute
        return this;
    }</pre>
<p></p>
<p>
    而onProgressUpdate实际上是在这个handle中被调用了。
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">/**
     * KJTaskExecutor内部Handler，用来发送后台计算进度更新消息和计算完成消息
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
    有关本类的完整代码可以查看<a target="_blank" href="https://github.com/kymjs/KJFrameForAndroid/blob/master/KJLibrary/src/org/kymjs/aframe/core/KJTaskExecutor.java" rel="nofollow">https://github.com/kymjs/KJFrameForAndroid/blob/master/KJLibrary/src/org/kymjs/aframe/core/KJTaskExecutor.java</a><br/>
</p>