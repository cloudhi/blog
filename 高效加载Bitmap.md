#��Ч����Bitmap����Ч�������OOM������
<p>
    ������дAndroid�����ʱ�򣬿϶����õ��ܶ�ͼƬ����ô����ͼƬ��ѹ��������Ȼ�Ǳز����١�ΪʲôҪѹ��������������ⲻ����ǿ���ˣ�ÿ���������ѧϰAndroid��ʱ��϶�����֪����ôһ��ԭ�����Ǳ�д��Ӧ�ó�������һ������ڴ����ƣ�����JAVA�����C����NDK����ʱ��������һ���ڴ��С������ռ���˹��ߵ��ڴ�����׳���OOM(OutOfMemory)�쳣�������������ڴ��Ƕ��٣����ǿ���ͨ������Runtime.getRuntime().maxMemory()������֤һ�¡�<br/><br/>����Ϊ�ܵ��ڴ��С������һ�ؼ�ԭ����ʵ��ֹ���ԭ������һ��1M��ͼƬ��һ��10k��ͼƬ��������ٶȱ�ȻҲ�ǲ�ͬ�İɣ��� �����Ŀؼ���Сֻ��40*40���صĴ�С��ֻ��Ϊ����ʾһ������ͼ����ʱ���һ��1024*768���ص�ͼƬ��ȫ���ص��ڴ�����Ȼ�ǲ�ֵ�õģ�������Ƕ����ͼƬ��ѹ������<br/><br/>BitmapFactory������ṩ�˶������(decodeByteArray, decodeFile, decodeResource��)���ڴ���Bitmap�������ǿ��Ը���ͼƬ����Դѡ����ʵķ�����Ȼ����Щ������Ϊ�Ѿ���ȡ��bitmap�����ڴ棬��ʱ�����һ�ŷǳ����ͼƬ�ͻᵼ��OOM���֡�Ϊ�ˣ�ÿһ�ֽ����������ṩ��һ��BitmapFactory.Options����������ͨ�������������inJustDecodeBounds��������Ϊtrue�Ϳ����ý���������ֹΪbitmap�����ڴ棬����������ú�BitmapFactory�ķ���ֵҲ������һ��Bitmap���󣬶���null����ȻBitmap��null�ˣ�����BitmapFactory.Options��outWidth��outHeight��outMimeType���Զ��ᱻ��ֵ��ʹ��������������ǿ����ڼ���ͼƬ֮ǰ�ͻ�ȡ��ͼƬ�ĳ���ֵ�����ͣ��Ӷ����������ͼƬ����ѹ����<br/>
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">  BitmapFactory.Options options = new BitmapFactory.Options();  
    options.inJustDecodeBounds = true;  
    BitmapFactory.decodeFile(pathName, options);
    int h = options.outHeight;  
    int w = options.outWidth;  
    String type = options.outMimeType;</pre>
<p></p>
<p>
    ��ô֪����ͼƬ�Ŀ�ߣ�Ҫ���ѹ���أ�BitmapFactory.Options��һ��inSampleSize���ԣ����intֵ��ʾͼƬ��ԭ��߱�Ϊ1/inSampleSize�������ԭͼ��1024*768��inSampleSize=2����ôѹ����ͼƬ�ͱ����512*384��<br/>���BitmapFactory.Options���ú��ʵ�inSampleSizeֵ�����Ҽǵý�inJustDecodeBounds���û�false���ٵ���һ��BitmapFactory��Ӧ�Ĵ���Bitmap�ķ���������Options���룬�Ϳ��Եõ�ѹ�����ͼƬ�ˡ�
</p>
<p>
    ������һ����ѡ�Կ�ԴAndroidӦ�ÿ������KJFrameForAndroid�е�һ�δ���
</p>
<pre class="brush:java;toolbar: true; auto-links: false;"> /**
     * ͼƬѹ������ʹ��Options�ķ�����
     * 
     * @ʹ�÷��� ������Ҫ��Options��inJustDecodeBounds��������Ϊtrue��BitmapFactory.decodeһ��ͼƬ��
     *       Ȼ��Options��ͬ�����Ŀ�Ⱥ͸߶�һ�𴫵ݵ����������С�
     *       ֮����ʹ�ñ������ķ���ֵ����������BitmapFactory.decode����ͼƬ��
     * 
     * @explain BitmapFactory����bitmap�᳢��Ϊ�Ѿ�������bitmap�����ڴ�
     *          ����ʱ�ͻ�����׵���OOM���֡�Ϊ��ÿһ�ִ����������ṩ��һ����ѡ��Options����
     *          �������������inJustDecodeBounds��������Ϊtrue�Ϳ����ý���������ֹΪbitmap�����ڴ�
     *          ������ֵҲ������һ��Bitmap���� ����null����ȻBitmap��null�ˣ�����Options��outWidth��
     *          outHeight��outMimeType���Զ��ᱻ��ֵ��
     * @param reqWidth
     *            Ŀ����
     * @param reqHeight
     *            Ŀ��߶�
     */
    public static BitmapFactory.Options calculateInSampleSize(
            final BitmapFactory.Options options, int reqWidth, int reqHeight) {
        // ԴͼƬ�ĸ߶ȺͿ��
        final int height = options.outHeight;
        final int width = options.outWidth;
        int inSampleSize = 1;
        if (height &gt; reqHeight || width &gt; reqWidth) {
            // �����ʵ�ʿ�ߺ�Ŀ���ߵı���
            final int heightRatio = Math.round((float) height
                    / (float) reqHeight);
            final int widthRatio = Math.round((float) width / (float) reqWidth);
            // ѡ���͸�����С�ı�����ΪinSampleSize��ֵ���������Ա�֤����ͼƬ�Ŀ�͸�
            // һ��������ڵ���Ŀ��Ŀ�͸ߡ�
            inSampleSize = heightRatio &lt; widthRatio ? heightRatio : widthRatio;
        }
        // ����ѹ������
        options.inSampleSize = inSampleSize;
        options.inJustDecodeBounds = false;
        return options;
    }</pre>
<p></p>
<p>
    ���ϵķ����ʺ�ʹ���ڶ�ȡһ��δ֪��Դ��ͼƬʱʹ�ã���Ϊ�㲻֪�����δ֪��ԴͼƬ�Ĵ�С����ô����һ�ַ����������Ѿ������ڴ��ͼƬ�����Ѿ������ڴ��ͼƬ��ѹ���Ժ����±��浽���أ��Ӷ����԰�һ��ԭ��1M��С��ͼƬ���һ��10K��ͼƬ��<br/>���ַ����ĺ���˼�������Ƚ�ͼƬת��һ�������������¼�������byte�����С��ͨ������bitmap�����compress��������ͼƬ��һ��ѹ���Լ���ʽ��������byte�����С������ѹ����Ŀ���С�ȶԣ��ó�ѹ�����ʣ�������Bitmap�����ŷ��������ż������ѹ�����ʣ��Ӷ��õ�ѹ����ķ�����<br/>�������Ǽ�������KJFrameForAndroid����е���һ�δ��룺
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">/**
     * ͼƬѹ����������ʹ��compress�ķ�����
     * 
     * @explain ���bitmap����Ĵ�СС��maxSize����������
     * @param bitmap
     *            Ҫѹ����ͼƬ
     * @param maxSize
     *            ѹ����Ĵ�С����λkb
     */
    public static void imageZoom(Bitmap bitmap, double maxSize) {
        // ��bitmap���������У����ڻ��bitmap�Ĵ�С����ʵ�ʶ�ȡ��ԭ�ļ�Ҫ��
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        // ��ʽ�������������
        bitmap.compress(Bitmap.CompressFormat.JPEG, 100, baos);
        byte[] b = baos.toByteArray();
        // ���ֽڻ���KB
        double mid = b.length / 1024;
        // ��ȡbitmap��С ����������С�Ķ��ٱ�
        double i = mid / maxSize;
        // �ж�bitmapռ�ÿռ��Ƿ�����������ռ� ���������ѹ�� С����ѹ��
        if (i &gt; 1) {
            // ����ͼƬ �˴��õ�ƽ���� ������͸߶�ѹ������Ӧ��ƽ������
            // �����ֿ�߲��䣬���ź�Ҳ�ﵽ�����ռ�ÿռ�Ĵ�С��
            bitmap = scale(bitmap, bitmap.getWidth() / Math.sqrt(i),
                    bitmap.getHeight() / Math.sqrt(i));
        }
    }
/***
     * ͼƬ�����ŷ���
     * 
     * @param src
     *            ��ԴͼƬ��Դ
     * @param newWidth
     *            �����ź���
     * @param newHeight
     *            �����ź�߶�
     */
    public static Bitmap scale(Bitmap src, double newWidth, double newHeight) {
        // ��¼src�Ŀ��
        float width = src.getWidth();
        float height = src.getHeight();
        // ����һ��matrix����
        Matrix matrix = new Matrix();
        // �������ű���
        float scaleWidth = ((float) newWidth) / width;
        float scaleHeight = ((float) newHeight) / height;
        // ��ʼ����
        matrix.postScale(scaleWidth, scaleHeight);
        // �������ź��ͼƬ
        return Bitmap.createBitmap(src, 0, 0, (int) width, (int) height,
                matrix, true);
    }</pre>
<p></p>
<p>
    <br/><br/>
</p>