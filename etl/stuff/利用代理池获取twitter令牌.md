Twitter封堵免费API获取数据之后，RSSHub的twitter路由有一段时间无法使用。直到DIYgod大佬的[这个提交](https://github.com/DIYgod/RSSHub/commit/9032153c7de4c9ca189482d495696986e9795106)才以访客账户+Web-API的形式回归。

这个方法需要获取访客账户的OAuth Token以及secret，然后通过Web-API获取数据。因为是访客账户，请求次数有限，自然时常429。RSSHub也提供了批量生成的脚本，然而仍需要手动填写相关的env，而且同一个ip大量获取OAuth Token似乎会被制裁（坏的时候几十个token一天就全429了😭）

于是试图使用代理池来获取token，并对RSSHub进行一些丑陋的修改，在twitter route失败时自动更新token。

[仓库地址](https://github.com/CrackTC/xtoken)
