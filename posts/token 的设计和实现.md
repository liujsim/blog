token 的设计和实现

#### overview

关于 token 使用的讨论 [询问服务器 token 的实现方案](https://www.v2ex.com/t/202801)

token 用于服务器端的身份认证，通常提供给前端如 APP 进行身份验证

安全防护的目的：
主动调用

微信公众平台： 获取 AccessToken 需要使用 https 协议，json 数据格式，UTF-8 编码，数据包不需要加密,AccessToken需要用CorpID和Secret来换取，不同的Secret会返回不同的AccessToken。正常情况下AccessToken有效期为7200秒，有效期内重复获取返回相同结果；有效期内有接口交互（包括获取AccessToken的接口），会自动续期。

请求：GET 方式

    https://qyapi.weixin.qq.com/cgi-bin/gettoken?corpid=id&corpsecret=secrect

    
正确的 json 返回结果

    {   "access_token": "accesstoken000001",   "expires_in": 7200 }

得到 token 后调用其他接口

     https://qyapi.weixin.qq.com/cgi-bin/department/create?access_token=ACCESS_TOKEN

频率限制

当你获取到AccessToken时，你的应用就可以成功调用企业号后台所提供的各种接口以管理或访问企业号后台的资源或给企业号成员发消息。
为了防止企业应用的程序错误而引发企业号服务器负载异常，默认情况下，每个企业号调用接口都有一定的频率限制，当超过此限制时，调用对应接口会收到相应错误码。

#### mock

功能分析

1. token 如果过期，如何实现自动续期 （假设为 120 分钟），session 包含自动续期机制，如何手动实现

2. token 的生成算法，一般可以用用户ID（可以包含设备相关的 id）和时间戳加密，如果能成功解密而且时间没有过期，就认为登录成功

3. token 的保存：

   服务端： token 是临时数据，没有持久化的必要，使用缓存即可，存放token和激活时间，每次访问API都刷新一下token的激活时间。超时的时候删掉就好了。（类似 map ，key 存放 token，value 存放时间戳，每次访问都更新时间戳，到期会自动删除。或者用户退出登录就马上删除）

   客户端： url 或者 http header
   

4. 根据场景区分过期时间，要看app的设计需求了，如果是强安全的，像银行，证券类，每次放几分钟就超时的就设短一点。如果是聊天社交类的，你可以设一个月或者2周。每请求一次，就重设过期时间

5. 其他操作： token 的更新：修改密码时清除该用户在后台（内存、数据库等）的token记录。


#### 安全 point

功能分析
1. 限制请求次数
   leak bucket  [What's a good rate limiting algorithm?][1]
   [请求速率限制问题](http://blog.gssxgss.me/not-a-simple-problem-rate-limiting/)
2. 防止请求被篡改
   设计：加密参数得到一个sign值，然后服务器端再次加密参数，和sign值
   防止 cracker 根据请求格式推算出参数格式，模拟请求调用其它资源。可以将参数按升序排列，然后得到 md5 值（原字符串加私钥得到 md5），作为参数传到服务端，服务端对比再次根据参数计算得到 md5 值，如果相同则说明没有被篡改
   [百度open api 实现][2]
3. 防止重放攻击 （重复提交）
   noonce

4. https
    https只能保证网络上数据被截获了是破解不了的。 但是如果客户端被破解，https有啥用。。。，cracker 是破解他人的账户而不是自己的。在客户端通过 fiddler 中间人攻击属于
   浏览器的 F12 网络工具可以嗅探加密前的数据,fiddler 中间人攻击截获数据
5. acl
   验证码，判断请求 host ，只有指定 host 才能访问（通常用于内部接口形式）

其它参考推荐 [RESTful接口签名认证实现机制](http://bbs.zone.jd.com/forum.php?mod=viewthread&tid=138)

[1]:http://stackoverflow.com/questions/667508/whats-a-good-rate-limiting-algorithm/668327#668327

[2]:http://developer.baidu.com/wiki/index.php?title=%E7%AD%BE%E5%90%8D%E8%AE%A1%E7%AE%97%E7%AE%97%E6%B3%95

 

