#��ThoughtWorks������ĸ���
*����ܻ��һ�������⣬ThoughtWorks��������Ŀ�����Ŵ�Ҷ������ˡ�*
<p>
    ��ĿҪ����������
</p>
<p>
    1. ������˵��������ͬ����������Ҫ������Ǹ�λ��������3��5��7��<br/>2. ��100��ѧ���ĳ�һ�ӣ�Ȼ��˳������<br/>3. ѧ������ʱ��������������ǵ�һ����������3���ı�������ô����˵�����֣���Ҫ˵Fizz��<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ������������ǵڶ�����������5���ı�������ôҪ˵Buzz��<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ������������ǵ�������������7���ı�������ôҪ˵Whizz��<br/>4. ѧ������ʱ�������������ͬʱ�������������ı�������£�ҲҪ���⴦��<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; �����һ���������͵ڶ����������ı�������ô����˵�����֣�����Ҫ˵FizzBuzz, �Դ����ơ�<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ���ͬʱ�������������ı�������ôҪ˵FizzBuzzWhizz��<br/>5. ѧ������ʱ������������ְ����˵�һ������������ôҲ����˵�����֣�����Ҫ˵��Ӧ�ĵ��ʣ�<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ���籾���е�һ����������3����ôҪ��13��ͬѧӦ��˵Fizz��<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ��������а����˵�һ������������ô���Թ���3�͹���4��<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ����Ҫ��35��ͬѧֻ��Fizz������BuzzWhizz��
</p>
<p>
    �ڹ���֮ǰ�ͺ�ϲ��û��ȥѧУ��ACM��վˢ���棬���ڹ��������������������˼����Ŀ�ˣ��������俴��ThoughtWorks����������⣬˲�����һ��˵���ˢ��ʱ�ĸо���
</p>
<p>
    ����һ��android����Ա�������Ѿ���һ���û����C�����ˣ����ǿ��������Ŀ��ʱ�򣬵�һ��Ӧ������C����ȥʵ�֣��Ͼ��������㷨��Ŀ������C����ȥʵ�ֵġ�
</p>
<p>
    �������ҵ�һ��д�Ĵ��루����C����C��java����java�Ĵ����񣬻�ϣ����Ҳ�Ҫ�²ۣ���ʱ��û����C�����ˣ���
</p>
<pre class="brush:cpp;toolbar: true; auto-links: false;">#include&lt;stdio.h&gt;
int main(void)
{
    int a,b,c;
    int i;
    //��Ϊ���������ܱ�0�����Դ˴����Զ�0�ļ��
    printf(&quot;��������������&quot;);
    scanf(&quot;%d%d%d&quot;,&amp;a,&amp;b,&amp;c);
    printf(&quot;\n&quot;);

    //����С������ҪС������
    for(i = 1; i &lt;a;i++){
        printf(&quot;%d\n&quot;,i);
    }

    //�ж�
    for(i = a; i &lt;=100; i++)
    {
        switch(checked(a,b,c,i))
        {
        case 1:
            printf(&quot;Fizz\n&quot;);
            break;
        case 2:
            printf(&quot;Buzz\n&quot;);
            break;
        case 3:
            printf(&quot;Whizz\n&quot;);
            break;
        case 0:
            checked_multiple(a,b,c,i);
            printf(&quot;\n&quot;);
            break;
        }
    }
    return 0;
}

//��ⱶ��
void checked_multiple(int a, int b, int c, int n)
{
    int boolean = 0;
    if (n%a == 0){
        printf(&quot;Fizz&quot;);
        boolean = 1;
    }
    if (n%b == 0){
        printf(&quot;Buzz&quot;);
        boolean = 1;
    }
    if(n%c == 0){
        printf(&quot;Whizz&quot;);
        boolean = 1;
    }
    if(!boolean){
        printf(&quot;%d&quot;,n);
    }
}

//�����λ����ͬ�����
int checked(int a, int b, int c, int n)
{
    int prev;   //ʮλ
    int next;   //��λ
    if (n &gt;10){
        prev = n/10;
        next = n%10;
    }else{
        prev = 0;
    }
    if (a == prev || a == next){
        return 1;
    }
    if (b == prev || b == next){
        return 2;
    }
    if (c == prev || c == next){
        return 3;
    }
    return 0;
}</pre>
<p></p>
<p>
    �ڵ�һ��д��������ĸ�����ǣ��ұ��ˡ�<br/>
</p>
<p>
    �ұ�Ĳ����񵱳��Ǹ�һ����Ч�ʣ�һ��������������Լ������ڵ��ң����ǵĸ������<span style="background-color: rgb(255, 255, 255); color: rgb(255, 0, 0);"><strong>���ܵ�ģ�黯���������ԡ�</strong></span>
</p>
<p>
    ���������ǰ���Լ���һ����һ��main������ʮ����ʮ�д��룬�뾡һ�а취��ѹ����������ȥ��������Ŀ��
</p>
<p>
    �Ҳ�֪���ǲ�����Ϊ�Ӵ�����java���ǲ�����Ϊ��������ǲ�����Ϊ����̫�����Ŀ��ԭ�����ڵ��Լ����ǵĸ�����Ǳ�����ο����ҵĴ��룬�ҵĴ���������ø���ĳ��ϡ�
</p>
<p>
    ���ǣ��������µĸĶ����ڼ䷢���Լ�����Ŀ����⻹����Щ���������˵���ǽ����ǵ�һ���������֣�
</p>
<pre class="brush:cpp;toolbar: true; auto-links: false;">#include&lt;stdio.h&gt;
int main(void)
{
    int a,b,c;
    int i,j;
    //��Ϊ���������ܱ�0�����Դ˴����Զ�0�ļ��
    printf(&quot;���������������ö��ŷָ�����&quot;);
    scanf(&quot;%d,%d,%d&quot;,&amp;a,&amp;b,&amp;c);
    printf(&quot;\n&quot;);

    for(i = 1; i &lt; a; i++){
        printf(&quot;%d\n&quot;,i);
    }

    //�ж�
    for(i = a; i &lt;=100; i++){
        j = 0;
         if (checked_fizz(a,i)){
            printf(&quot;Fizz&quot;);
         }else{
            j += checked_multiple2(a,i,&quot;Fizz&quot;);
            j += checked_multiple2(b,i,&quot;Buzz&quot;);
            j += checked_multiple2(c,i,&quot;Whizz&quot;);
            if (!j){
                printf(&quot;%d&quot;,i);
            }
        }
        printf(&quot;\n&quot;);
    }
    return 0;
}
//��⺬�е�һ������
int checked_fizz(int a, int n){
    if (n/10 == a || n%10 == a){
        return 1;
    }else{
        return 0;
    }
}
//��ⱶ��
int checked_multiple2(int a, int n,char* str){
    if (n%a == 0){
        printf(&quot;%s&quot;,str);
        return 1;
    }else{
        return 0;
    }
}</pre>
<p></p>
<p>
    <span style="color: rgb(255, 0, 0);">�� ���ұ�֤����Ч����ߵĴ��룬���Ҹұ�֤���ǿ������κ���ʹ�õĴ��룬��ʹ��Ŀ�������ı䣺������������ֱ��kkkk,����jjj,�ٻ��������Ķ����� �ҵĺ���ģ���ǲ���Ҫ�ı�ģ��ҵ��߼��ж��ǲ���Ҫ�ı�ġ���ʹ��Ŀ�ٶ�����������֣��ҵĺ����������ɲ��䣬����Ҫ�ı�Ľ�����main�����ж�jֵ�� �ۼӲ���������</span>
</p>
<p>
    �������Ҷ����������ܽᣬҲ���Ҷ����Լ�������ı��һ�λعˡ�
</p>
<p>
    <span style="font-size: 16px; color: rgb(255, 0, 0);">��һֱ��Ϊ����ֻ����ʶ���Լ��Ĺ�ȥ�ı䣬����Ϊ��δ�����õĸı䡣</span><br/>
</p>
<p>
    <br/>
</p>