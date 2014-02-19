#### github page jekyll 解析

* github page自带有jekyll解析器，除非你上传了名为.nojekyll的文件组织github自动解析。
* github page如果解析错误就会发送解析错误的邮件提示你，但不要指望邮件提示具体什么错误，用本地环境测试吧。
* github page 环境
* github page的jekyll版本可以通过[versionts.json](http://pages.github.com/versions.json)查看，通常本地解析时候出现的问题可以通过搭建相同的环境来测试。

#### markdown 语法

github page有些私有markdown写法，可以查阅[mastering markdown](http://guides.github.com/overviews/mastering-markdown/)

#### markdown html混用

github page嵌入gist的script标签形式时需要注意markdown和html混合使用不能一行闭合标签


    <script src="https://gist.github.com/defims/9064063.js"></script>

是不会解析的，使用chrome dev tools可以看到标签未闭合，正确写法是

    <script src="https://gist.github.com/defims/9064063.js">
    </script>

其他html标签也有类似问题

{% highlight ruby %}
def show
  @widget = Widget(params[:id])
  respond_to do |format|
    format.html # show.html.erb
    format.json { render json: @widget }
  end
end
{% endhighlight %}
