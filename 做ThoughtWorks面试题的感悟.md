#做ThoughtWorks面试题的感悟
*最近很火的一道面试题，ThoughtWorks的面试题目，相信大家都看到了。*
<p>
    题目要求是这样的
</p>
<p>
    1. 你首先说出三个不同的特殊数，要求必须是个位数，比如3、5、7。<br/>2. 让100个学生拍成一队，然后按顺序报数。<br/>3. 学生报数时，如果所报数字是第一个特殊数（3）的倍数，那么不能说该数字，而要说Fizz；<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 如果所报数字是第二个特殊数（5）的倍数，那么要说Buzz；<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 如果所报数字是第三个特殊数（7）的倍数，那么要说Whizz。<br/>4. 学生报数时，如果所报数字同时是两个特殊数的倍数情况下，也要特殊处理，<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 比如第一个特殊数和第二个特殊数的倍数，那么不能说该数字，而是要说FizzBuzz, 以此类推。<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 如果同时是三个特殊数的倍数，那么要说FizzBuzzWhizz。<br/>5. 学生报数时，如果所报数字包含了第一个特殊数，那么也不能说该数字，而是要说相应的单词，<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 比如本例中第一个特殊数是3，那么要报13的同学应该说Fizz。<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 如果数字中包含了第一个特殊数，那么忽略规则3和规则4，<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 比如要报35的同学只报Fizz，不报BuzzWhizz。
</p>
<p>
    在工作之前就很喜欢没事去学校的ACM网站刷题玩，现在工作后很少在做这种有意思的题目了，这次无意间看到ThoughtWorks的这道面试题，瞬间又找回了当初刷题时的感觉。
</p>
<p>
    我是一名android程序员，尽管已经有一年多没有碰C语言了，但是看到这个题目的时候，第一反应还是用C语言去实现，毕竟当初的算法题目都是用C语言去实现的。
</p>
<p>
    以下是我第一遍写的代码（这种C不像C，java不像java的代码风格，还希望大家不要吐槽，长时间没有用C语言了）：
</p>
<pre class="brush:cpp;toolbar: true; auto-links: false;">#include&lt;stdio.h&gt;
int main(void)
{
    int a,b,c;
    int i;
    //因为报数不可能报0，所以此处忽略对0的检测
    printf(&quot;请输入三个数：&quot;);
    scanf(&quot;%d%d%d&quot;,&amp;a,&amp;b,&amp;c);
    printf(&quot;\n&quot;);

    //比最小的数还要小不考虑
    for(i = 1; i &lt;a;i++){
        printf(&quot;%d\n&quot;,i);
    }

    //判断
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

//检测倍数
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

//检测与位数相同的情况
int checked(int a, int b, int c, int n)
{
    int prev;   //十位
    int next;   //个位
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
    在第一遍写完后，我最大的感想就是：我变了。<br/>
</p>
<p>
    我变的不再像当初那个一心求效率，一心求代码行数的自己，现在的我，考虑的更多的是<span style="background-color: rgb(255, 255, 255); color: rgb(255, 0, 0);"><strong>功能的模块化，与重用性。</strong></span>
</p>
<p>
    如果是两年前的自己，一定会一个main函数，十几二十行代码，想尽一切办法的压缩代码行数去完成这个题目。
</p>
<p>
    我不知道是不是因为接触到了java，是不是因为面向对象，是不是因为做了太多的项目的原因，现在的自己考虑的更多的是别人如何看懂我的代码，我的代码如何适用更多的场合。
</p>
<p>
    于是，有了如下的改动（期间发现自己对题目的理解还出了些差错，第五项说的是仅考虑第一个特殊数字）
</p>
<pre class="brush:cpp;toolbar: true; auto-links: false;">#include&lt;stdio.h&gt;
int main(void)
{
    int a,b,c;
    int i,j;
    //因为报数不可能报0，所以此处忽略对0的检测
    printf(&quot;请输入三个数（用逗号分隔）：&quot;);
    scanf(&quot;%d,%d,%d&quot;,&amp;a,&amp;b,&amp;c);
    printf(&quot;\n&quot;);

    for(i = 1; i &lt; a; i++){
        printf(&quot;%d\n&quot;,i);
    }

    //判断
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
//检测含有第一个数字
int checked_fizz(int a, int n){
    if (n/10 == a || n%10 == a){
        return 1;
    }else{
        return 0;
    }
}
//检测倍数
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
    <span style="color: rgb(255, 0, 0);">我 不敢保证这是效率最高的代码，但我敢保证这是可以让任何人使用的代码，即使题目的条件改变：比如输出的文字变成kkkk,或者jjj,再或者其他的东西， 我的函数模块是不需要改变的，我的逻辑判断是不需要改变的。即使题目再多添加特殊数字，我的函数功能依旧不变，所需要改变的仅仅是main函数中对j值的 累加操作次数。</span>
</p>
<p>
    以上是我对于这道题的总结，也是我对于自己这两年改变的一次回顾。
</p>
<p>
    <span style="font-size: 16px; color: rgb(255, 0, 0);">我一直认为，人只有认识到自己的过去改变，才能为了未来更好的改变。</span><br/>
</p>
<p>
    <br/>
</p>