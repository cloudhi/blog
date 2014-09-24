#GIT使用入门，图形界面版
*本文参考自：http://my.oschina.net/longxuu/blog/141699*
*本文是我由一个只使用过SVN，从未使用过GIT的菜鸟，到能使用eclipse正常提交代码到git@oschina的学习过程。*
<p>
    首先，如果你想使用git<a href="http://my.oschina.net/osadmin" class="referer" target="_blank">@oschina</a> ，你的电脑上必须先有git工具：你可以从这里获取谷歌提供的git.exe <a href="http://git-scm.com/" rel="nofollow">http://git-scm.com/</a> 当然，如果你能熟练通过命令行操作git，那么这一个工具完全够你使用了。当然，如果那样，大神也不用再看这篇博客。所以，我推荐再下载一个tortoisegit <a href="http://code.google.com/p/tortoisegit/" rel="nofollow">http://code.google.com/p/tortoisegit/</a> （需要先安装git，在安装tortoisegit）。
</p>
<p>
    接下来我们就开始搭建本地与Git@OSC的桥梁了。<br/>
</p>
<p>
    <br/>首先，在开始菜单找到<img src="http://static.oschina.net/uploads/space/2013/1229/185617_yasd_863548.png"/>
</p>
<p>
    运行后点击generate，创建密钥（会等一段时间）得到后保存公钥和私钥。
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2013/1229/185828_0Xlr_863548.png"/>
</p>
<p>
    这时候就可以进入http://git.oschina.net/keys/new，添加自己的公钥，
</p>
<p>
    <strong><span style="color:#E53333;font-size:14px;"><img src="http://static.oschina.net/uploads/space/2013/0701/225048_0v88_244889.jpg" height="300" width="748"/></span></strong>
</p>
<p>
    此时就可以在git@oschina创建一个项目了，复制项目地址，在电脑本地选择一个同步项目的目录，最好是空的，然后右键：在这里创建版本库，不要勾选，确定。
</p>
<p>
    然后右键，tortoisegit--&gt;setting<br/>
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2013/1229/190635_suUM_863548.png"/>
</p>
<p>
    <br/> 
</p>
<p>
    远端是自动生成的，URL就是复制的git<a href="http://my.oschina.net/osadmin" class="referer" target="_blank">@oschina</a> &nbsp;putty密钥是本地的私钥<br/>
</p>
<p>
    <br/> 
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2013/0701/225854_yCU7_244889.jpg" height="245" width="454"/>
</p>
<p>
    设置完成后，回到同步项目的目录，右键，pull，就可以把远端代码拉去到本地了
</p>
<p>
    然后右键菜单：Git提交-&gt;master，写注释，点确定
</p>
<p>
    最后右键菜单：TortoiseGit-&gt;推送，直接点确定
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2013/0701/231012_AHlF_244889.jpg" height="535" width="555"/>
</p>
<p>
    /////////////////////////////////////////////////////////////////////////////////////////////////////
</p>
<p>
    以上就完成了在文件夹中的操作，下面介绍eclipse插件的用法
</p>
<p>
    如果你想在eclipse中使用git，需要安装git插件 <a href="http://www.eclipse.org/egit/" rel="nofollow">http://www.eclipse.org/egit/</a> （eclipse插件安装方式请自行搜索）
</p>
<p>
    插件安装好后切换到git仓库视图
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2013/1229/191842_7FTm_863548.png"/>
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2013/1229/192027_lHv1_863548.png"/><br/>
</p>
<p>
    选择中间的克隆一个git仓库
</p>
<p>
    URI就是git@oschina复制的地址，host与路径自动生成，protocol选择相应的类型（https或者ssh），用户名或密码就是这里的<a href="https://git.oschina.net/" rel="nofollow">https://git.oschina.net/</a> 登录名和密码（如果不行就试试将邮箱设置为用户名）
</p>
<p>
    <img src="http://static.oschina.net/uploads/space/2013/1229/192119_cjx9_863548.png"/>
</p>
<p>
    <br/>
</p>
<p>
    最后一路next就完成从远端pull的过程了
</p>
<p>
    上传代码的时候，右键工程名，先team-&gt;commit 然后再team-&gt;push才能完成上传<br/>
</p>