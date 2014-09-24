#Android布局技巧之屏幕适配
*本文主要讲解android开发中：如何在代码中做到屏幕适配*
<p>
    在做android应用开发的时候，遇到最头疼的问题之一大概就是屏幕适配了，那么除了常用的使用不同分辨率的layout布局文件外，有没有更方便的办法呢？答案是肯定的。
</p>
<p>
    首先看图片，一个简单的登录界面
</p>
<p>
    <img src="http://static.oschina.net/uploads/img/201405/30091559_onXR.jpg" height="800" width="480"/>
</p>
<p>
    <br/>
</p>
<p>
    这个登录界面的布局比较简单大家一看就明白了两个TextView和EditView的嵌套组合成的输入框。
</p>
<p>
    那么，我要说的问题是：在480*800的屏幕上，我们看到的是输入框占全屏宽度（设置了边距但宽度仍是占满父控件）到了800*1600、600*1200（我瞎编的分辨率，大家别见怪）换了一个大屏幕的手机，难道还是铺满父控件吗，一个长度是头像几十倍的账号密码输入框显然是不美观的。
</p>
<p>
    更多的解决办法是在项目中建立不同分辨率的layout文件夹（像系统默认提供的drawable文件夹一样）来适应不同的屏幕，但是这样做太复杂了，现在屏幕分辨率还不算多，如果再过几年，魅族再出个奇葩分辨率屏幕，步步高、三星再出个盾牌大小的手机，我们难道再添加不同屏幕的layout文件夹吗？这当然是不可取的。那么最好的办法就是从代码中去适配屏幕了，这也是本文所要讲的。
</p>
<p>
    首先说一下ViewGroup，在ViewGroup类中有一个叫LayoutParams的内部类。我们可以通过构造方法创建一个layoutparams，当然更好的办法是所有的View都有一个方法叫getLayoutParams()，调用view的这个方法，我们可以获取到这个View所在的父控件的layoutparams属性。
</p>
<p>
    这里需要注意一点，就是View的父控件几乎不可能是直接的ViewGroup，而更多的可能是LinearLayout.LayoutParams，RelativeLayout.LayoutParams等布局的属性，这时直接去用ViewGroup的LayoutParams去操作是没有用的，我们需要强转成父控件类型的LayoutParams。
</p>
<p>
    此时我们可以通过获取到屏幕的宽度，计算出希望控件占有屏幕的百分比（我通常是用宽度乘以80%），自己手动设置layoutparams.leftMargin与layoutparams.rightMargin这两个值。从字面上大家应该就能理解了，这两个值就是左边距和右边距。此时控件就是以百分比的形式占在屏幕的中间了。
</p>
<p>
    最后，一定不要忘记了最关键的一步，把我们修改过的params重新设置给View：view.setlayoutparams(params);这样就完成我们在代码中的屏幕适配了。
</p>
<p>
    下面看看一段示例代码吧
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">private void screenAdapter() {
        // 登陆按钮占屏幕宽度的70%，输入框宽度与登陆按钮一样
        int screenW = DensityUtils.getScreenW(this);
        RelativeLayout.LayoutParams btnLoginParams = (LayoutParams) mBtnLogin
                .getLayoutParams();
        btnLoginParams.leftMargin = (int) ((screenW * 0.25) / 2);
        btnLoginParams.rightMargin = (int) ((screenW * 0.25) / 2);
        mBtnLogin.setLayoutParams(btnLoginParams);

        mLayoutUid.measure(0, 0);
        int layoutUidHeight = mLayoutUid.getMeasuredHeight();
        // 头像在输入框正上方空白处居中
        RelativeLayout.LayoutParams headImgParams = (LayoutParams) mImgHead
                .getLayoutParams();
        int screenH = DensityUtils.getScreenH(this);
        headImgParams.height = (int) (screenH / 6.5);
        headImgParams.width = headImgParams.height;
        int headImgHeight = headImgParams.height;
        // （屏幕的中心-27-输入框的高度-头像图片的高度）&amp;divide;2
        int inputTop = screenH / 2 - 27 - layoutUidHeight;
        headImgParams.topMargin = (inputTop - headImgHeight) / 2;
        mImgHead.setLayoutParams(headImgParams);
    }</pre>
<p></p>
<p>
    <br/>
</p>
<p>
    <br/>
</p>
<p>
    <br/>
</p>
<p>
    <br/>
</p>
<p>
    <br/>
</p>
<p>
    <br/>
</p>
<p>
    <br/>
</p>