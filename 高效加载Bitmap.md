#高效加载Bitmap，有效解决出现OOM的问题
<p>
    我们在写Android程序的时候，肯定会用到很多图片。那么对于图片的压缩处理自然是必不可少。为什么要压缩？我想这个问题不必在强调了，每个人在最初学习Android的时候肯定都会知道这么一个原因：我们编写的应用程序都是有一个最大内存限制，其中JAVA程序和C程序（NDK调用时）共享这一块内存大小，程序占用了过高的内存就容易出现OOM(OutOfMemory)异常。至于这个最大内存是多少，我们可以通过调用Runtime.getRuntime().maxMemory()方法验证一下。<br/><br/>正因为受到内存大小限制这一关键原因（其实不止这个原因，我想一张1M的图片和一张10k的图片，载入的速度必然也是不同的吧）。 如果你的控件大小只有40*40像素的大小，只是为了显示一张缩略图，这时候把一张1024*768像素的图片完全加载到内存中显然是不值得的，因此我们都会对图片做压缩处理。<br/><br/>BitmapFactory这个类提供了多个方法(decodeByteArray, decodeFile, decodeResource等)用于创建Bitmap对象，我们可以根据图片的来源选择合适的方法。然而这些方法会为已经读取的bitmap分配内存，这时如果是一张非常大的图片就会导致OOM出现。为此，每一种解析方法都提供了一个BitmapFactory.Options参数，可以通过将这个参数的inJustDecodeBounds属性设置为true就可以让解析方法禁止为bitmap分配内存，但是如此设置后BitmapFactory的返回值也不再是一个Bitmap对象，而是null。虽然Bitmap是null了，但是BitmapFactory.Options的outWidth、outHeight和outMimeType属性都会被赋值。使用这个技巧让我们可以在加载图片之前就获取到图片的长宽值和类型，从而根据情况对图片进行压缩。<br/>
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">  BitmapFactory.Options options = new BitmapFactory.Options();  
    options.inJustDecodeBounds = true;  
    BitmapFactory.decodeFile(pathName, options);
    int h = options.outHeight;  
    int w = options.outWidth;  
    String type = options.outMimeType;</pre>
<p></p>
<p>
    那么知道了图片的宽高，要如何压缩呢？BitmapFactory.Options有一个inSampleSize属性，这个int值表示图片的原宽高变为1/inSampleSize倍，如果原图是1024*768，inSampleSize=2，那么压缩后图片就变成了512*384。<br/>最后将BitmapFactory.Options设置合适的inSampleSize值，并且记得将inJustDecodeBounds设置回false，再调用一次BitmapFactory相应的创建Bitmap的方法，并把Options传入，就可以得到压缩后的图片了。
</p>
<p>
    这里有一个节选自开源Android应用开发框架KJFrameForAndroid中的一段代码
</p>
<pre class="brush:java;toolbar: true; auto-links: false;"> /**
     * 图片压缩处理（使用Options的方法）
     * 
     * @使用方法 首先你要将Options的inJustDecodeBounds属性设置为true，BitmapFactory.decode一次图片。
     *       然后将Options连同期望的宽度和高度一起传递到到本方法中。
     *       之后再使用本方法的返回值做参数调用BitmapFactory.decode创建图片。
     * 
     * @explain BitmapFactory创建bitmap会尝试为已经构建的bitmap分配内存
     *          ，这时就会很容易导致OOM出现。为此每一种创建方法都提供了一个可选的Options参数
     *          ，将这个参数的inJustDecodeBounds属性设置为true就可以让解析方法禁止为bitmap分配内存
     *          ，返回值也不再是一个Bitmap对象， 而是null。虽然Bitmap是null了，但是Options的outWidth、
     *          outHeight和outMimeType属性都会被赋值。
     * @param reqWidth
     *            目标宽度
     * @param reqHeight
     *            目标高度
     */
    public static BitmapFactory.Options calculateInSampleSize(
            final BitmapFactory.Options options, int reqWidth, int reqHeight) {
        // 源图片的高度和宽度
        final int height = options.outHeight;
        final int width = options.outWidth;
        int inSampleSize = 1;
        if (height &gt; reqHeight || width &gt; reqWidth) {
            // 计算出实际宽高和目标宽高的比率
            final int heightRatio = Math.round((float) height
                    / (float) reqHeight);
            final int widthRatio = Math.round((float) width / (float) reqWidth);
            // 选择宽和高中最小的比率作为inSampleSize的值，这样可以保证最终图片的宽和高
            // 一定都会大于等于目标的宽和高。
            inSampleSize = heightRatio &lt; widthRatio ? heightRatio : widthRatio;
        }
        // 设置压缩比例
        options.inSampleSize = inSampleSize;
        options.inJustDecodeBounds = false;
        return options;
    }</pre>
<p></p>
<p>
    以上的方法适合使用在读取一个未知来源的图片时使用，因为你不知道这个未知来源图片的大小，那么还有一种方法是用在已经载入内存的图片，对已经载入内存的图片做压缩以后重新保存到本地，从而可以把一张原本1M大小的图片变成一张10K的图片。<br/>这种方法的核心思想是首先将图片转成一个输出流，并记录输出流的byte数组大小，通过调用bitmap对象的compress方法，对图片做一次压缩以及格式化，并将byte数组大小与期望压缩的目标大小比对，得出压缩比率，并调用Bitmap的缩放方法，缩放计算出的压缩比率，从而得到压缩后的方法。<br/>下面我们继续来看KJFrameForAndroid框架中的另一段代码：
</p>
<pre class="brush:java;toolbar: true; auto-links: false;">/**
     * 图片压缩方法：（使用compress的方法）
     * 
     * @explain 如果bitmap本身的大小小于maxSize，则不作处理
     * @param bitmap
     *            要压缩的图片
     * @param maxSize
     *            压缩后的大小，单位kb
     */
    public static void imageZoom(Bitmap bitmap, double maxSize) {
        // 将bitmap放至数组中，意在获得bitmap的大小（与实际读取的原文件要大）
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        // 格式、质量、输出流
        bitmap.compress(Bitmap.CompressFormat.JPEG, 100, baos);
        byte[] b = baos.toByteArray();
        // 将字节换成KB
        double mid = b.length / 1024;
        // 获取bitmap大小 是允许最大大小的多少倍
        double i = mid / maxSize;
        // 判断bitmap占用空间是否大于允许最大空间 如果大于则压缩 小于则不压缩
        if (i &gt; 1) {
            // 缩放图片 此处用到平方根 将宽带和高度压缩掉对应的平方根倍
            // （保持宽高不变，缩放后也达到了最大占用空间的大小）
            bitmap = scale(bitmap, bitmap.getWidth() / Math.sqrt(i),
                    bitmap.getHeight() / Math.sqrt(i));
        }
    }
/***
     * 图片的缩放方法
     * 
     * @param src
     *            ：源图片资源
     * @param newWidth
     *            ：缩放后宽度
     * @param newHeight
     *            ：缩放后高度
     */
    public static Bitmap scale(Bitmap src, double newWidth, double newHeight) {
        // 记录src的宽高
        float width = src.getWidth();
        float height = src.getHeight();
        // 创建一个matrix容器
        Matrix matrix = new Matrix();
        // 计算缩放比例
        float scaleWidth = ((float) newWidth) / width;
        float scaleHeight = ((float) newHeight) / height;
        // 开始缩放
        matrix.postScale(scaleWidth, scaleHeight);
        // 创建缩放后的图片
        return Bitmap.createBitmap(src, 0, 0, (int) width, (int) height,
                matrix, true);
    }</pre>
<p></p>
<p>
    <br/><br/>
</p>