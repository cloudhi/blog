#Android���ּ���֮��Ļ����
*������Ҫ����android�����У�����ڴ�����������Ļ����*
<p>
    ����androidӦ�ÿ�����ʱ��������ͷ�۵�����֮һ��ž�����Ļ�����ˣ���ô���˳��õ�ʹ�ò�ͬ�ֱ��ʵ�layout�����ļ��⣬��û�и�����İ취�أ����ǿ϶��ġ�
</p>
<p>
    ���ȿ�ͼƬ��һ���򵥵ĵ�¼����
</p>
<p>
    <img src="http://static.oschina.net/uploads/img/201405/30091559_onXR.jpg" height="800" width="480"/>
</p>
<p>
    <br/>
</p>
<p>
    �����¼����Ĳ��ֱȽϼ򵥴��һ��������������TextView��EditView��Ƕ����ϳɵ������
</p>
<p>
    ��ô����Ҫ˵�������ǣ���480*800����Ļ�ϣ����ǿ������������ռȫ����ȣ������˱߾൫�������ռ�����ؼ�������800*1600��600*1200����Ϲ��ķֱ��ʣ���ұ���֣�����һ������Ļ���ֻ����ѵ������������ؼ���һ��������ͷ��ʮ�����˺������������Ȼ�ǲ����۵ġ�
</p>
<p>
    ����Ľ���취������Ŀ�н�����ͬ�ֱ��ʵ�layout�ļ��У���ϵͳĬ���ṩ��drawable�ļ���һ��������Ӧ��ͬ����Ļ������������̫�����ˣ�������Ļ�ֱ��ʻ�����࣬����ٹ����꣬�����ٳ�������ֱ�����Ļ�������ߡ������ٳ������ƴ�С���ֻ��������ѵ�����Ӳ�ͬ��Ļ��layout�ļ������⵱Ȼ�ǲ���ȡ�ġ���ô��õİ취���ǴӴ�����ȥ������Ļ�ˣ���Ҳ�Ǳ�����Ҫ���ġ�
</p>
<p>
    ����˵һ��ViewGroup����ViewGroup������һ����LayoutParams���ڲ��ࡣ���ǿ���ͨ�����췽������һ��layoutparams����Ȼ���õİ취�����е�View����һ��������getLayoutParams()������view��������������ǿ��Ի�ȡ�����View���ڵĸ��ؼ���layoutparams���ԡ�
</p>
<p>
    ������Ҫע��һ�㣬����View�ĸ��ؼ�������������ֱ�ӵ�ViewGroup��������Ŀ�����LinearLayout.LayoutParams��RelativeLayout.LayoutParams�Ȳ��ֵ����ԣ���ʱֱ��ȥ��ViewGroup��LayoutParamsȥ������û���õģ�������Ҫǿת�ɸ��ؼ����͵�LayoutParams��
</p>
<p>
    ��ʱ���ǿ���ͨ����ȡ����Ļ�Ŀ�ȣ������ϣ���ؼ�ռ����Ļ�İٷֱȣ���ͨ�����ÿ�ȳ���80%�����Լ��ֶ�����layoutparams.leftMargin��layoutparams.rightMargin������ֵ���������ϴ��Ӧ�þ�������ˣ�������ֵ������߾���ұ߾ࡣ��ʱ�ؼ������԰ٷֱȵ���ʽռ����Ļ���м��ˡ�
</p>
<p>
    ���һ����Ҫ��������ؼ���һ�����������޸Ĺ���params�������ø�View��view.setlayoutparams(params);��������������ڴ����е���Ļ�����ˡ�
</p>
<p>
    ���濴��һ��ʾ�������
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">private void screenAdapter() {
        // ��½��ťռ��Ļ��ȵ�70%������������½��ťһ��
        int screenW = DensityUtils.getScreenW(this);
        RelativeLayout.LayoutParams btnLoginParams = (LayoutParams) mBtnLogin
                .getLayoutParams();
        btnLoginParams.leftMargin = (int) ((screenW * 0.25) / 2);
        btnLoginParams.rightMargin = (int) ((screenW * 0.25) / 2);
        mBtnLogin.setLayoutParams(btnLoginParams);

        mLayoutUid.measure(0, 0);
        int layoutUidHeight = mLayoutUid.getMeasuredHeight();
        // ͷ������������Ϸ��հ״�����
        RelativeLayout.LayoutParams headImgParams = (LayoutParams) mImgHead
                .getLayoutParams();
        int screenH = DensityUtils.getScreenH(this);
        headImgParams.height = (int) (screenH / 6.5);
        headImgParams.width = headImgParams.height;
        int headImgHeight = headImgParams.height;
        // ����Ļ������-27-�����ĸ߶�-ͷ��ͼƬ�ĸ߶ȣ�&amp;divide;2
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