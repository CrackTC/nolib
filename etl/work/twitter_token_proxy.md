# 利用代理池获取twitter令牌（已失效）

Twitter 封堵免费 API 获取数据之后，RSSHub 的 twitter 路由有一段时间无法使用。直到 DIYgod 大佬的[这个提交](https://github.com/DIYgod/RSSHub/commit/9032153c7de4c9ca189482d495696986e9795106)才以访客账户 + Web-API 的形式回归。

这个方法需要获取访客账户的 OAuth Token 以及 secret，然后通过 Web-API 获取数据。因为是访客账户，请求次数有限，自然时常 429。RSSHub 也提供了批量生成的脚本，然而仍需要手动填写相关的 env，而且同一个 ip 大量获取 OAuth Token 似乎会被制裁（坏的时候几十个 token 一天就全 429 了😭）

于是试图使用代理池来获取 token，并对 RSSHub 进行一些丑陋的修改，在 twitter route 失败时自动更新 token。

~~[仓库地址](https://github.com/CrackTC/xtoken)~~ (Archived)

> guest account 已被封禁
