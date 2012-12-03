.. DNSPod API documentation master file, created by
   sphinx-quickstart on Thu Aug 23 11:47:21 2012.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

.. toctree::
    :maxdepth: 2

DNSPod用户API文档
=================

API开发规范
------------    

1. 关于滥用：
    以下这些，但并不限于以下这些行为都被视为滥用：
        a. 短时间内大量添加、删除、修改、刷新域名或者记录，或者设置记录状态。
        b. 记录内容没有任何改变的刷新，比如动态DNS中IP没变也请求修改记录。
        c. 其它没有提到的，但会给系统带来压力的请求行为。

2. 禁止登录：
    如果账号尝试登录错误次数在5分钟内超过30次则会被禁止登录一个小时，写程序的时候特别是动态客户端的时候必须判断是否登录成功，如果不成功必须马上停止尝试。如果使用动态客户端，在网页上修改了密码后务必到客户端修改，否则将不可用。

3. 关于封禁：
    由于滥用API将会导致账号在API中封禁，但并不影响在官方网站上的使用，封禁一定的时间后会自动解除，一般需要一个小时后才会解封，所以请小心操作，不要拿API进行大量测试。

4. 请求地址：
    请求的地址为 https://dnsapi.cn/ 开头的地址，基于SSL安全传输。为了安全必须用https开头的地址，否则将有可能被封禁。

5. 请求方法：
    只支持POST方法请求数据，用其它方法会提示相关错误。

6. 关于编码：
    请用UTF-8编码进行数据传输，返回的数据也是UTF-8编码的。

7. 关于UserAgent：
    请求的时候必须设置UserAgent，如果不设置或者设置为不合法的（比如设置为浏览器的）也会导致账号被封禁API。
    UserAgent的格式必须为：程序英文名称/版本 (联系邮箱)
    比如：MJJ DDNS Client/1.0.0 (shallwedance@126.com)

8. 关于安全：
    用户需要输入用户邮箱和密码才能进行下一步的操作，如果需要在客户端上或数据库中保存用户的信息，必须加密后保存，不得明文保存在文件中。

9. 技术支持：
    请登录论坛 http://discussdns.com/forum-45-1.html 提问。




最近更新
---------
2012-07-27
    * 增加D令牌验证相关的说明
2012-05-07
    * 获取记录列表接口增加按子域名(sub_domain)获取记录
    * 增加Domain.Info接口
    * 增加Record.Info接口
2012-03-22
    * 增加设置域名搜索引擎推送接口
    * 增加设置域名启用国内URL转发接口
2012-03-14
    * User.Detail接口增加了nick 返回，意为用户称呼
    * User.Modify接口增加了 nick 参数，意为用户称呼
2012-03-09
    * 添加和修改宕机监控接口增加了 less_notice 参数
    * 获取警告信息接口增加了 switch_log 返回
2012-02-24
    * 增加了宕机监控相关的API

公共参数
---------
所有的接口都需要这些参数，在此统一说明，下面的接口将不再列出。这些参数必须和接口参数一起提交。
    * login_email 用户帐号，必选
    * login_password 用户密码，必选
    * format {json,xml} 返回的数据格式，可选，默认为xml，建议用json
    * lang {en,cn} 返回的错误语言，可选，默认为en，建议用cn
    * error_on_empty {yes,no} 没有数据时是否返回错误，可选，默认为yes，建议用no
    * user_id 用户的ID，可选，仅代理接口需要， 用户接口不需要提交此参数

共通返回
---------
这些返回在所有的接口都有可能返回，它们是所有接口共通的，在此统一说明，下面的接口将不再列出。
    * -1 登陆失败
    * -2 API使用超出限制
    * -3 不是合法代理 (仅用于代理接口)
    * -4 不在代理名下 (仅用于代理接口)
    * -7 无权使用此接口
    * -8 登录失败次数过多，账号被暂时封禁
    * -99 此功能暂停开放，请稍候重试
    * 1 操作成功
    * 2 只允许POST方法
    * 3 未知错误
    * 6 用户ID错误 (仅用于代理接口)
    * 7 用户不在您名下 (仅用于代理接口)

D令牌验证相关
--------------
对于已经开启D令牌的用户，在请求API时需要增加如下两个参数：
    * login_code D令牌生成的随机验证码，必选
    * login_remember {yes,no} 是否记住验证码，可选，默认为yes

说明：由于D令牌的随机码每30秒变换一次，所以建议用户在第一次请求API的时候选择记住验证码。选择记住验证码后，除了返回所请求的数据以外，还会发一个名为't' + user_id 的cookie，形如：t123456，该cookie的有效期为1个月，以后再请求API可以带上这个cookie，而无须再提交login_code和login_remember参数。

对于开启了D令牌的用户，某些重要操作可能会返回以下错误码，在此统一说明，下面的接口将不再列出。
    * 50 您开启了D令牌，需要验证码
    * 51 没有开启D令牌
    * 52 验证码不正确
    * 53 您已经开启了D令牌
    * 54 域名所有者开启了D令牌，您也需要开启D令牌才能管理该域名

获取API版本号
--------------
接口地址：
    * https://dnsapi.cn/Info.Version
HTTP请求方式：
    * POST
请求参数：
    * 公共参数 
响应代码：
    * 共通返回

示例::
    
    curl -X POST https://dnsapi.cn/Info.Version -d 'login_email=api@dnspod.com&login_password=password&format=json'

返回参考：

    * JSON::

        {
            "status": {
                "code": "1",
                "message": "4.6",
                "created_at": "2012-09-10 11:20:39"
            }
        }


帐户相关
========
    
获取帐户信息
-------------
接口地址：
    * https://dnsapi.cn/User.Detail
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
响应代码：
    * 共通返回

示例::
    
    curl -X POST https://dnsapi.cn/User.Detail -d 'login_email=api@dnspod.com&login_password=password&format=json'

返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-10 11:22:26"
            },
            "info": {
                "user": {
                    "id": "625033",
                    "email": "api@dnspod.com",
                    "status": "enabled",
                    "email_verified": "no",
                    "telephone_verified": "yes",
                    "agent_pending": false,
                    "real_name": "",
                    "user_type": "personal",
                    "telephone": "15012345678",
                    "im": "10000000",
                    "nick": "DNSPod 先生",
                    "balance": "0",
                    "smsbalance": "4"
                }
            }
        }

修改资料
--------
接口地址：
    * https://dnsapi.cn/User.Modify
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * real_name 真实姓名，如果用户类型是企业，则为公司名称
    * nick 用户称呼，用于与用户联系时称呼用户
    * telephone 用户手机号码
    * im 用户qq
响应代码：
    * 共通返回
    * 8 手机号码不正确
    * 9 用户qq不正确

示例::
    
    curl -X POST https://dnsapi.cn/User.Modify -d 'login_email=api@dnspod.com&login_password=password&format=json&im=10000000'

返回参考：

    * JSON::

        {
            "status": {
                "code":"1",
                "message":"Action completed successful",
                "created_at":"2012-08-24 13:34:56"
            }
        }

修改密码
---------
接口地址：
    * https://dnsapi.cn/Userpasswd.Modify
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * old_password 旧密码
    * new_password 新密码
响应代码：
    * 共通返回
    * 8 旧密码错误
    * 9 新密码错误

示例::
    
    curl -X POST https://dnsapi.cn/Userpassword.Modify -d 'login_email=api@dnspod.com&login_password=password&format=json&old_password=old_password&new_password=new_password'

返回参考：

    * JSON::

        {
            "status": {
                "code":"1",
                "message":"Action completed successful",
                "created_at":"2012-08-24 13:45:27"
            }
        }

修改邮箱
---------
接口地址：
    * https://dnsapi.cn/Useremail.Modify
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * old_email 旧邮箱
    * new_email 新邮箱
    * password 当前密码
响应代码：
    * 共通返回
    * 8 旧邮箱错误
    * 9 新邮箱错误
    * 10 当前密码错误

示例:: 

    curl -X POST https://dnsapi.cn/Useremail.Modify -d 'login_email=api@dnspod.com&login_password=password&format=json&old_email=api1@dnspod.com&new_email=api@dnspod.com&password=password'   

返回参考：

    * JSON::
        
        {
            "status": {
                "code":"1",
                "message":"Action completed successful",
                "created_at":"2012-08-24 14:49:41"
            }
        }

        
获取手机验证码
---------------
接口地址：
    * https://dnsapi.cn/Telephoneverify.Code
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * telephone 用户手机号码
响应代码：
    * 共通参数
    * 4 用户已经通过手机验证
    * 5 手机号码非法

示例::
    
    curl -X POST https://dnsapi.cn/Telephoneverify.Code -d 'login_email=api@dnspod.com&login_password=password&format=json&telephone=18600000000'

返回参考：

    * JSON::
        
        {
            "status": {
                "code":"4",
                "message":"Telephone is verified",
                "created_at":"2012-08-24 15:57:21"
            }
        }

        {
            "status": {
                "code":"1",
                "message":"Action completed successful",
                "created_at":"2012-11-23 16:01:52"
            }, 
            "user": {
                "verify_code":"676479",
                "verify_desc":"\u8bf7\u4f7f\u7528 18600000000 \u7f16\u8f91\u77ed\u4fe1\uff0c\u5c06 676479 \u53d1\u9001\u81f3\u53f7\u7801  159 6183 3568\u3002"
            }
        }

获取用户日志
-------------
接口地址：
    * https://dnsapi.cn/User.Log
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
响应代码：
    * 共通返回

示例::

    curl -X POST https://dnsapi.cn/User.Log -d 'login_email=api@dnspod.com&login_password=password&format=json'

返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-10 11:29:36"
            },
            "log": 
            [
                "2012-09-04 13:56:24: 111.111.111.111 登陆 成功",
                "2012-08-30 17:01:47: 111.111.111.111 登陆 成功",
                "2012-08-29 22:12:35(API): (111.111.111.111) 添加域名 api2.com",
                "2012-08-29 21:59:55: (111.111.111.111) 添加域名 api1.com",
                "2012-08-29 21:59:45: (111.111.111.111) 添加域名 apiapi.com",
                "2012-08-29 21:59:30: 111.111.111.111 登陆 成功",
                "2012-08-24 15:49:53: 111.111.111.111 登陆 成功",
            ]
        }
    

域名相关
========

添加新域名
-----------
接口地址：
    * https://dnsapi.cn/Domain.Create
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain 域名, 没有 www, 如 dnspod.com
    * group_id 域名分组ID，可选参数
    * is_mark {yes|no} 是否星标域名，可选参数
响应代码：
    * 共通返回
    * 6 域名无效
    * 7 域名已存在
    * 11 域名已经存在并且是其它域名的别名
    * 12 域名已经存在并且您没有权限管理
    * 41 网站内容不符合DNSPod解析服务条款，域名添加失败

示例::

    curl -X POST https://dnsapi.cn/Domain.Create -d 'login_email=api@dnspod.com&login_password=password&domain=api2.com&format=json'

返回参考：

    * JSON::
        
        {
            "status": {
                "code":"1",
                "message":"Action completed successful",
                "created_at":"2012-08-29 22:12:35"
            },
            "domain": {
                "id":"1992403",
                "punycode":"api2.com",
                "domain":"api2.com"
            }
        }


获取域名列表
-------------
接口地址：
    * https://dnsapi.cn/Domain.List
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * type 域名权限种类，可选参数，默认为'all'。包含以下类型：
        * all：所有域名
        * mine：我的域名
        * share：共享给我的域名
        * ismark：星标域名
        * pause：暂停域名
        * vip：VIP域名
        * recent：最近操作过的域名
        * share_out：我共享出去的域名
    * offset 记录开始的偏移，第一条记录为 0，依次类推，可选参数
    * length 共要获取的记录的数量，比如获取20条，则为20，可选参数
    * group_id 分组ID，获取指定分组的域名，可选参数
注意事项：
    * 如果账户中的域名数量超过了3000，将会强制分页并且只返回前3000条，这时需要通过 offset 和 length 参数去获取其它域名。
响应代码：
    * 共通返回
    * 6 记录开始的偏移无效
    * 7 共要获取的记录的数量无效
    * 9 没有任何域名

示例::
    
    curl -X POST https://dnsapi.cn/Domain.List -d 'login_email=api@dnspod.com&login_password=password&format=json'

返回参考：

   * JSON::

        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-10 11:51:31"
            },
            "info": {
                "domain_total": 1,
                "all_total": 1,
                "mine_total": 1,
                "share_total": 0,
                "vip_total": 0,
                "ismark_total": 0,
                "pause_total": 0,
                "error_total": 1,
                "lock_total": 0,
                "spam_total": 0,
                "share_out_total": 0
            },
            "domains": [
                {
                    "id": 1992403,
                    "name": "api2.com",
                    "grade": "D_Free",
                    "grade_title": "免费套餐",
                    "status": "enable",
                    "ext_status": "dnserror",
                    "records": "2",
                    "group_id": "1",
                    "is_mark": "no",
                    "remark": "",
                    "is_vip": "no",
                    "searchengine_push": "yes",
                    "beian": "no",
                    "created_on": "2012-08-29 22:12:35",
                    "updated_on": "2012-08-29 22:12:35",
                    "ttl": "600",
                    "owner": "api@dnspod.com"
                }
            ]
        }    

删除域名
---------
接口地址：
    * https://dnsapi.cn/Domain.Remove
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain，分别对应域名ID和域名，提交其中一个即可
响应代码：
    * 共通返回
    * -15 域名已被封禁
    * 6 域名ID错误
    * 7 域名已锁定
    * 8 VIP域名不可以删除
    * 9 非域名所有者

示例::

    curl -X POST https://dnsapi.cn/Domain.Remove -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=1992403'
    
返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-12 11:09:31"
            }
        }

设置域名状态
-------------
接口地址：
    * https://dnsapi.cn/Domain.Status
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain，分别对应域名ID和域名，提交其中一个即可
    * status {enable, disable} 域名状态
响应代码：
    * 共通返回
    * -15 域名已被封禁
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 7 域名被锁定
    * 8 非域名所有者

示例::

    curl -X POST https://dnsapi.cn/Domain.Status -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2058967&status=disable'

返回参考：

    * JSON::
            
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-12 12:02:04"
            }
        }

获取域名信息
-------------
接口地址：
    * https://dnsapi.cn/Domain.Info
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain，分别对应域名ID和域名，提交其中一个即可
响应代码：
    * 共通返回
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 8 非域名所有者

示例::

    curl -X POST https://dnsapi.cn/Domain.Info  -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079'

返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-12 12:10:16"
            },
            "domain": {
                "id": "2059079",
                "name": "api4.com",
                "punycode": "api4.com",
                "grade": "D_Free",
                "grade_title": "免费套餐",
                "status": "pause",
                "ext_status": "dnserror",
                "records": "9",
                "group_id": "1",
                "is_mark": "no",
                "remark": "",
                "is_vip": "no",
                "searchengine_push": "yes",
                "beian": "no",
                "user_id": "625033",
                "created_on": "2012-09-12 12:05:46",
                "updated_on": "2012-09-12 12:06:12",
                "ttl": "600",
                "owner": "api@dnspod.com"
            }
        } 


获取域名日志
-------------
接口地址：
    * https://dnsapi.cn/Domain.Log
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain，分别对应域名ID和域名，提交其中一个即可
响应代码：
    * 共通返回
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 8 非域名所有者

示例::
    
    curl -X POST https://dnsapi.cn/Domain.Log  -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079'

返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-18 17:24:23"
            },
            "log": [
                "2012-09-12 12:07:05: (111.111.111.111) 启用解析 NS 记录 默认 线路 @ 值 f1g1ns1.dnspod.net.",
                "2012-09-12 12:07:04: (111.111.111.111) 启用解析 NS 记录 默认 线路 @ 值 f1g1ns2.dnspod.net. ",
                "2012-09-12 12:07:02: (111.111.111.111) 暂停解析 NS 记录 默认 线路 @ 值 f1g1ns2.dnspod.net. ",
                "2012-09-12 12:06:57: (111.111.111.111) 暂停解析 NS 记录 默认 线路 @ 值 f1g1ns1.dnspod.net. ",
                "2012-09-12 12:06:33(API): (111.111.111.111) 暂停 域名解析",
                "2012-09-12 12:06:12: (111.111.111.111) 添加 CNAME 记录 默认 线路 pop 值 mail.api4.com. ",
                "2012-09-12 12:06:12: (111.111.111.111) 添加 A 记录 默认 线路 shop 值 64.144.7.55 ",
                "2012-09-12 12:06:12: (111.111.111.111) 添加 CNAME 记录 默认 线路 smtp 值 mail.api4.com. ",
                "2012-09-12 12:06:12: (111.111.111.111) 添加 CNAME 记录 默认 线路 webmail 值 webmail.secureserver.net. ",
                "2012-09-12 12:06:11: (111.111.111.111) 添加 A 记录 默认 线路 www 值 64.144.7.51 ",
                "2012-09-12 12:06:11: (111.111.111.111) 添加 A 记录 默认 线路 ftp 值 64.144.7.51 ",
                "2012-09-12 12:06:11: (111.111.111.111) 添加 CNAME 记录 默认 线路 e 值 email.secureserver.net. ",
                "2012-09-12 12:05:46: (111.111.111.111) 添加新域名 api4.com api@dnspod.com(625033)"
            ]
        } 

设置域名搜索引擎推送
---------------------
接口地址：
    * https://dnsapi.cn/Domain.Searchenginepush
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain，分别对应域名ID和域名，提交其中一个即可
    * status {yes,no} 是否推送
响应代码：
    * 共通返回
    * -15 域名已被封禁
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 7 域名被锁定
    * 8 非域名所有者

示例::

    curl -X POST https://dnsapi.cn/Domain.Searchenginepush -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079&status=yes'
    
返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-18 17:28:44"
            }
        }

设置域名国内URL转发(废弃 不更新)
---------------------------------
接口地址：

HTTP请求方式：
    * POST
请求参数：
    * 公共参数
示例

返回参考：


添加域名共享
-------------
接口地址：
    * https://dnsapi.cn/Domainshare.Create
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain，分别对应域名ID和域名，提交其中一个即可
    * email 要共享到的邮箱
    * mode {r,rw}共享模式，r (只读)　或 rw (读取、修改)，默认为 r
    * sub_domain 子域名共享，如：www、bbs等。如果要共享整个域名，则无需提交此参数

响应代码：
    * 共通返回
    * -15 域名已被封禁
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 7 要共享到的邮箱错误
    * 8 要共享到的邮箱不存在
    * 9 共享已经存在
    * 10 共享数量已经到达上限

示例::

    curl -X POST https://dnsapi.cn/Domainshare.Create -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079&email=otheruser@dnspod.com&mode=rw'
    
返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-18 17:47:21"
            }
        }
    
域名共享列表
-------------
接口地址：
    * https://dnsapi.cn/Domainshare.List
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain，分别对应域名ID和域名，提交其中一个即可
响应代码：
    * 共通返回
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 7 没有共享记录
响应代码：
    * 共通返回
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 7 没有共享记录

示例::
    
    curl -X POST https://dnsapi.cn/Domainshare.List -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079'

返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-18 17:51:50"
            },
            "share": [
                {
                    "share_to": "yizerowu@dnspod.com",
                    "mode": "rw",
                    "status": "enabled"
                }
            ],
            "owner": "api@dnspod.com"
        }

修改域名共享
-------------
接口地址：
    * https://dnsapi.cn/Domainshare.Modify
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain，分别对应域名ID和域名，提交其中一个即可
    * email 被共享者的邮箱，原来是什么就提交什么，不能修改
    * mode {r,rw}共享模式，r (只读)　或 rw (读取、修改)，默认为 r
    * old_sub_domain 已经成功共享的子域名，如果只修改主域名共享，则无需提交此参数
    * new_sub_domain 要修改到的共享子域名
响应代码：
    * 共通返回
    * -15 域名已被封禁
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 7 要共享到的邮箱错误
    * 8 要共享到的邮箱不存在
    * 9 此邮箱的共享不存在

示例

1. 修改主域名共享状态，从 rw 修改为 r::
        
    curl -X POST https://dnsapi.cn/Domainshare.Modify -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079&email=yizerowu@dnspod.com&mode=r'
    
2. 修改子域名的共享状态，从rw 修改为 r::
            
    curl -X POST https://dnsapi.cn/Domainshare.Modify -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079&email=yizerowu@dnspod.com&mode=r&old_sub_domain=www&new_sub_domain=www'
    
3. 将主域名共享修改为子域名共享::

    curl -X POST https://dnsapi.cn/Domainshare.Modify -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079&email=yizerowu@dnspod.com&mode=rw&new_sub_domain=www'
    
4. 将子域名共享修改为主域名共享::

    curl -X POST https://dnsapi.cn/Domainshare.Modify -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079&email=yizerowu@dnspod.com&mode=rw&old_sub_domain=www'
    
5. 将www的子域名共享修改为bbs的子主域名共享::

    curl -X POST https://dnsapi.cn/Domainshare.Modify -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079&email=yizerowu@dnspod.com&mode=rw&old_sub_domain=www&new_sub_domain=bbs'
    
返回参考：

   * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-18 18:54:18"
            }
        } 

删除域名共享
-------------
接口地址：
    *  https://dnsapi.cn/Domainshare.Remove
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain，分别对应域名ID和域名，提交其中一个即可
    * email 被共享者的邮箱，原来是什么就提交什么，不能修改
响应代码：
    * 共通返回
    * -15 域名已被封禁
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 7 要共享到的邮箱错误
    * 8 要共享到的邮箱不存在
    * 9 此邮箱的共享不存在

示例::
    
    curl -X POST https://dnsapi.cn/Domainshare.Remove -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079&email=yizerowu@dnspod.com'

返回参考：

    * JSON::    
    
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-18 20:19:20"
            }
        }

域名过户
---------
接口地址：
    * https://dnsapi.cn/Domain.Transfer
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain，分别对应域名ID和域名，提交其中一个即可
    * email 被共享者的邮箱，原来是什么就提交什么，不能修改
响应代码：
    * 共通返回
    * -15 域名已被封禁
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 7 要过户到的账号邮箱错误
    * 8 要过户到的账号邮箱不存在
    * 9 不能过户给自己
    * 10 个人用户域名不能过户给企业账号
    * 11 企业用户域名不能过户给个人账号

示例::
    
    curl -X POST https://dnsapi.cn/Domainshare.Transfer -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079&email=yizerowu@dnspod.com'
    
返回参考：

    * JSON::    
    
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-18 20:21:33"
            }
        }

锁定域名
---------
接口地址：
    * https://dnsapi.cn/Domain.Lock
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 域名ID
    * days 要锁定的天数
响应代码：
    * 共通返回
    * -15 域名已被封禁
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 7 不是域名所有者或者没有权限
    * 8 锁定天数错误
    * 9 锁定天数超出限制
    * 21 域名已经被锁定

示例::
    
    curl -X POST https://dnsapi.cn/Domain.Lock -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079&days=3'

返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-18 20:31:13"
            },
            "lock": {
                "domain_id": 2059079,
                "lock_code": "fdd638",
                "lock_end": "2012-09-21"
            }
        }

锁定状态
---------
接口地址：
    * https://dnsapi.cn/Domain.Lockstatus
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain，分别对应域名ID和域名，提交其中一个即可
响应代码：
    * 共通返回
    * -15 域名已被封禁
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 7 域名没有锁定

示例::
    
    curl -X POST https://dnsapi.cn/Domain.Lockstatus -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079'
    
返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-18 20:35:04"
            },
            "lock": {
                "lock_status": "yes",
                "start_at": "2012-09-18",
                "end_at": "2012-09-21"
            }
        }

锁定解锁
---------
接口地址：
    * https://dnsapi.cn/Domain.Unlock
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain，分别对应域名ID和域名，提交其中一个即可
    * lock_code 域名解锁码，锁定的时候会返回
响应代码：
    * 共通返回
    * -15 域名已被封禁
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 7 不是域名所有者或者没有权限
    * 8 域名没有锁定
    * 9 解锁码错误

示例::
    
    curl -X POST https://dnsapi.cn/Domain.Unlock -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079&lock_code=fdd638'

返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-18 20:38:58"
            },
            "lock": {
                "lock_status": "yes",
                "start_at": "2012-09-18",
                "end_at": "2012-09-21"
            }
        }

域名绑定列表
-------------
接口地址：
    * https://dnsapi.cn/Domainalias.List
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain，分别对应域名ID和域名，提交其中一个即可
响应代码：
    * 共通返回
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 7 没有绑定记录

示例::
    
    curl -X POST https://dnsapi.cn/Domainalias.List -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079'

返回参考：

   * JSON::

        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-18 21:11:20"
            },
            "alias": [
                {
                    "id": "18737",
                    "domain": "dnspodapi.com"
                }
            ]
        } 


添加域名绑定
-------------
接口地址：
    * https://dnsapi.cn/Domainalias.Create
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id ，域名ID
    * domain 要绑定的域名，不带www.
响应代码：
    * 共通返回
    * -15 域名已被封禁
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 7 要绑定的域名错误
    * 8 要绑定的域名已经被添加
    * 9 要绑定的域名已经被绑定
    * 10 绑定数量已经到达上限

示例::
    
    curl -X POST https://dnsapi.cn/Domainalias.Create -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079&domain=dnspodapi.com'

返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-18 21:09:57"
            },
            "alias": {
                "id": "18737",
                "punycode": "dnspodapi.com"
            }
        }

删除域名绑定
-------------
接口地址：
    * https://dnsapi.cn/Domainalias.Remove
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain，分别对应域名ID和域名，提交其中一个即可
    * alias_id 绑定ID，绑定域名的时候会返回
响应代码：
    * 共通返回
    * -15 域名已被封禁
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 7 绑定ID错误

示例::
    
    curl -X POST https://dnsapi.cn/Domainalias.Remove -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079&alias_id=18737'

返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-18 21:15:20"
            }
        }

获取域名分组
-------------
接口地址：
    * https://dnsapi.cn/Domaingroup.List
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
响应代码：
    * 共通返回

示例::
    
    curl -X POST https://dnsapi.cn/Domaingroup.List -d 'login_email=api@dnspod.com&login_password=password&format=json'
    
返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-18 21:33:33"
            },
            "groups": [
                {
                    "group_id": 1,
                    "group_name": "默认分组",
                    "group_type": "system",
                    "size": 1
                },
                {
                    "group_id": 2,
                    "group_name": "经常修改",
                    "group_type": "system",
                    "size": null
                },
                {
                    "group_id": 3,
                    "group_name": "很少修改",
                    "group_type": "system",
                    "size": null
                },
                {
                    "group_id": 4,
                    "group_name": "即将到期",
                    "group_type": "system",
                    "size": null
                },
                {
                    "group_id": 5,
                    "group_name": "私人域名",
                    "group_type": "system",
                    "size": null
                },
                {
                    "group_id": 6,
                    "group_name": "公司域名",
                    "group_type": "system",
                    "size": null
                },
                {
                    "group_id": 7,
                    "group_name": "客户域名",
                    "group_type": "system",
                    "size": null
                },
                {
                    "group_id": 8,
                    "group_name": "与我共享",
                    "group_type": "system",
                    "size": null
                }
            ]
        }

说明：
    * 该接口只对VPI帐户有效，免费用户无法获取域名分组，免费用户会返回错误提示。
    
添加域名分组
-------------
接口地址：
    https://dnsapi.cn/Domaingroup.Create
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * group_name 分组名称
响应代码：
    * 共通返回
    * 7 分组名称错误
    * 8 分组名称已经存在
    * 9 分组数量超出限制

示例::
    
    curl -X POST https://dnsapi.cn/Domaingroup.List -d 'login_email=api@dnspod.com&login_password=password&format=json&group_name=dnspod'

返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-18 21:41:00"
            },
            "groups": {
                "id": "1985"
            }
        }

说明：
    * 该接口只对VPI帐户有效，免费用户无法获取域名分组，免费用户会返回错误提示。

修改域名分组
-------------
接口地址：
    https://dnsapi.cn/Domaingroup.Modify
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * group_id 分组ID
    * group_name 分组名称
响应代码：
    * 共通返回
    * 6 分组ID错误
    * 7 分组名称错误
    * 8 分组名称已经存在
    * 9 分组数量超出限制

示例::
    
    curl -X POST https://dnsapi.cn/Domaingroup.Modify -d 'login_email=api@dnspod.com&login_password=password&format=json&group_id=1985&group_name=dnspodgroup'

返回参考：

    * JSON::

        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-19 10:25:32"
            }
        }
    
删除域名分组
-------------
接口地址：
    * https://dnsapi.cn/Domaingroup.Remove
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * group_id 分组ID
响应代码：
    * 共通返回
    * 6 分组ID错误

示例::
    
    curl -X POST https://dnsapi.cn/Domaingroup.Remove -d 'login_email=api@dnspod.com&login_password=password&format=json&group_id=1985'

返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-19 10:45:45"
            }
        }
    
设置域名分组(更改域名所在分组)
------------------------------
接口地址：
    * https://dnsapi.cn/Domain.Changegroup
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain，分别对应域名ID和域名，提交其中一个即可
    * group_id 分组ID
响应代码：
    * 共通返回
    * 6 域名ID错误
    * 7 分组ID错误

示例::
    
    curl -X POST https://dnsapi.cn/Domain.Changegroup -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079&group_id=1985'
    
返回参考：

   * JSON::
    
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-23 17:33:01"
            }
        } 

说明：

    * 其它用户共享过来的域名全在“与我共享”系统分组下，不能更改
    * 只有域名所有者可以更改域名的分组，被共享者不能更改

设置域名星标
-------------
接口地址：
    * https://dnsapi.cn/Domain.Ismark
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain，分别对应域名ID和域名，提交其中一个即可
    * is_mark {yes | no}, 是否星标域名
响应代码：
    * 共通返回
    * 6 域名ID错误
示例::
    
    curl -X POST https://dnsapi.cn/Domain.Ismark -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079&is_mark=yes'

返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-23 17:42:02"
            }
        }

设置域名备注
-------------
接口地址：
    * https://dnsapi.cn/Domain.Remark
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain，分别对应域名ID和域名，提交其中一个即可
    * remark 域名备注，删除备注请提交空内容
响应代码：
    * 共通返回
    * 6 域名ID错误

示例::
    
    curl -X POST https://dnsapi.cn/Domain.Remark -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079&remark=这个域名需要备注一下'
    
返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-23 17:50:37"
            }
        }

获取域名权限
-------------
接口地址：
    * https://dnsapi.cn/Domain.Purview
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain，分别对应域名ID和域名，提交其中一个即可
响应代码：
    * 共通返回
    * 6 域名ID不正确

示例::
    
    curl -X POST https://dnsapi.cn/Domain.Purview -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2059079'
    
返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-23 17:51:25"
            },
            "purview": [
                {
                    "name": "URL转发条数",
                    "value": 10
                },
                {
                    "name": "NS记录条数",
                    "value": 99999
                },
                {
                    "name": "AAAA记录条数",
                    "value": 99999
                },
                {
                    "name": "SRV记录条数",
                    "value": 10
                },
                {
                    "name": "域名别名绑定个数",
                    "value": 3
                },
                {
                    "name": "域名锁定天数",
                    "value": 30
                },
                {
                    "name": "域名共享个数",
                    "value": 2
                },
                {
                    "name": "子域名级数",
                    "value": 3
                },
                {
                    "name": "泛解析级数",
                    "value": 2
                },
                {
                    "name": "负载均衡数量",
                    "value": 4
                },
                {
                    "name": "记录TTL最低",
                    "value": 120
                },
                {
                    "name": "混合泛解析支持",
                    "value": "no"
                },
                {
                    "name": "增强线路类型",
                    "value": "yes"
                },
                {
                    "name": "分省线路类型",
                    "value": "no"
                },
                {
                    "name": "分大洲线路类型",
                    "value": "no"
                }
            ]
        }

说明：
    * 获取成功后要在本地保存一份，不要操作一次获取一次

域名取回获取邮箱列表
---------------------
接口地址：
    * https://dnsapi.cn/Domain.Acquire
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain 要取回的域名
响应代码：
    * 共通返回
    * 6 域名不正确
    * 7 不支持中文域名
    * 8  域名不正确
    * 9 不支持免费.tk域名
    * 10 域名不存在
    * 11 域名已被封禁，不能取回
    * 12 锁定的域名不能取回
    * 13 个人用户不能取回企业用户域名
    * 14 企业用户不能取回个人用户域名
    * 15 获取不到邮箱，可能是网络错误或域名不支持

示例::
    
    curl -X POST https://dnsapi.cn/Domain.Acquire -d 'login_email=api@dnspod.com&login_password=password&format=json&domain=api4.com'
    
返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-23 18:00:05"
            },
            "emails": [
                "support@namecheap.com",
                "e31d739cb2824a5f80d7b90848a195d8.protect@whoisguard.com"
            ]
        }

域名取回发送验证码
--------------------
接口地址：
    *  https://dnsapi.cn/Domain.Acquiresend
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain 要取回的域名
    * email 域名取回邮箱列表中的一个邮箱
响应代码：
    * 共通返回
    * 6 域名不正确
    * 7 不支持中文域名
    * 8  域名不正确
    * 9 不支持免费.tk域名
    * 10 域名不存在
    * 11 域名已被封禁，不能取回
    * 12 锁定的域名不能取回
    * 13 个人用户不能取回企业用户域名
    * 14 企业用户不能取回个人用户域名
    * 15 获取不到邮箱，可能是网络错误或域名不支持
    * 16 邮箱参数错误

示例::
    
    curl -X POST https://dnsapi.cn/Domain.Acquiresend -d 'login_email=api@dnspod.com&login_password=password&format=json&domain=api4.com&email=support@namecheap.com'
    
返回参考：
    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-23 18:07:44"
            }
        }

验证域名取回的验证码
---------------------
接口地址：
    * https://dnsapi.cn/Domain.Acquirevalidate
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain 要取回的域名
    * code  发送到邮箱的验证码
响应代码：
    * 共通返回
    * 6 域名不正确
    * 7 不支持中文域名
    * 8  域名不正确
    * 9 不支持免费.tk域名
    * 10 域名不存在
    * 11 域名已被封禁，不能取回
    * 12 锁定的域名不能取回
    * 13 个人用户不能取回企业用户域名
    * 14 企业用户不能取回个人用户域名
    * 15 验证码不正确
    * 16 邮箱参数错误

示例::
    
    curl -X POST https://dnsapi.cn/Domain.Acquirevalidate -d 'login_email=api@dnspod.com&login_password=password&format=json&domain=api4.com&code=111000'
    
返回参考：

    * JSON::
            
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-23 18:12:44"
            }
        }

获取等级允许的记录类型(修改)
----------------------------
接口地址：
    *  https://dnsapi.cn/Record.Type
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_grade 域名等级，分别为：D_Free, D_Plus, D_Extra, D_Expert, D_Ultra，分别对应免费套餐、个人豪华、企业1、企业2、企业3
响应代码：
    * 共通返回
    * 6 等级代码不正确

示例::
    
    curl -X POST https://dnsapi.cn/Domain.Acquirevalidate -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_grade=D_Free'

返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-23 18:23:40"
            },
            "types": [
                "A",
                "CNAME",
                "MX",
                "TXT",
                "NS",
                "AAAA",
                "SRV",
                "URL"
            ]
        }    

获取等级允许的记录线路
-----------------------
接口地址：
    *  https://dnsapi.cn/Record.Line
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_grade 域名等级，分别为：D_Free, D_Plus, D_Extra, D_Expert, D_Ultra，分别对应免费套餐、个人豪华、企业1、企业2、企业3
    * domain_id 或 domain，分别对应域名ID和域名，提交其中一个即可
响应代码：
    * 共通返回
    * 6 等级代码不正确

示例::
    
    curl -X POST https://dnsapi.cn/Domain.Acquirevalidate -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_grade=D_Free&domain_id=2059079'

返回参考：

    * JSON::
        
            {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2012-09-23 18:29:58"
            },
            "lines": [
                "默认",
                "电信",
                "联通",
                "教育网",
                "移动",
                "铁通",
                "国内",
                "国外",
                "搜索引擎",
                "百度",
                "Google",
                "有道",
                "必应",
                "搜搜",
                "搜狗",
                "360搜索"
            ]
            }


记录相关
========
添加记录
---------
接口地址：
    * https://dnsapi.cn/Record.Create
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id  域名ID, 必选
    * sub_domain  主机记录, 如 www, 默认@，可选
    * record_type  记录类型，通过API记录类型获得，大写英文，比如：A, 必选
    * record_line  记录线路，通过API记录线路获得，中文，比如：默认, 必选
    * value  记录值, 如 IP:200.200.200.200, CNAME: cname.dnspod.com., MX: mail.dnspod.com., 必选
    * mx {1-20}  MX优先级, 当记录类型是 MX 时有效，范围1-20, MX记录必选
    * ttl {1-604800}  TTL，范围1-604800，不同等级域名最小值不同, 可选
响应代码：
    * 共通返回
    * -15 域名已被封禁
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 缺少参数或者参数错误
    * 7 不是域名所有者或者没有权限
    * 21 域名被锁定
    * 22 子域名不合法
    * 23 子域名级数超出限制
    * 24 泛解析子域名错误
    * 25 轮循记录数量超出限制
    * 26 记录线路错误
    * 27 记录类型错误
    * 30 MX 值错误，1-20
    * 31 URL记录数超出限制
    * 32 NS 记录数超出限制
    * 33 AAAA 记录数超出限制
    * 34 记录值非法
    * 36 @主机的NS纪录只能添加默认线路

示例::

    curl -X POST https://dnsapi.cn/Record.Create -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2317346&sub_domain=@&record_type=A&record_line=默认&value=1.1.1.1'
    
返回参考：

    * JSON::

        {
            "status": {
                "code":"1",
                "message":"Action completed successful",
                "created_at":"2012-11-23 22:17:47"
            },
            "record": {
                "id":"16894439",
                "name":"@",
                "status":"enable"
            }
        }

记录列表
---------
接口地址：
    * https://dnsapi.cn/Record.List
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 域名ID，必选
    * offset 记录开始的偏移，第一条记录为 0，依次类推，可选
    * length 共要获取的记录的数量，比如获取20条，则为20，可选
    * sub_domain 子域名，如果指定则只返回此子域名的记录，可选
响应代码：
    * 共通返回
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 7 记录开始的偏移无效
    * 8 共要获取的记录的数量无效
    * 9 不是域名所有者
    * 10 没有记录

注意事项：
    * 如果域名的记录数量超过了3000，将会强制分页并且只返回前3000条，这时需要通过 offset 和 length 参数去获取其它记录。

示例::

     curl -X POST https://dnsapi.cn/Record.List -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2317346'
    
返回参考：

    * JSON::

        {
            "status": {
                "code":"1",
                "message":"Action completed successful",
                "created_at":"2012-11-23 22:20:59"
            },
            "domain": {
                "id":2317346,
                "name":"testapi.com",
                "punycode":"testapi.com",
                "grade":"D_Plus",
                "owner":"api@dnspod.com"
            },
            "info": {
                "sub_domains":"3",
                "record_total":"3"
            },
            "records": [
                {
                    "id":"16894439",
                    "name":"@",
                    "line":"\u9ed8\u8ba4",
                    "type":"A",
                    "ttl":"600",
                    "value":"1.1.1.1",
                    "mx":"0",
                    "enabled":"1",
                    "status":"enabled",
                    "monitor_status":"",
                    "remark":"",
                    "updated_on":"2012-11-23 22:17:47"
                },
                {
                    "id":"16662141",
                    "name":"@",
                    "line":"\u9ed8\u8ba4",
                    "type":"NS",
                    "ttl":"600",
                    "value":"ns1.dnsv2.com.",
                    "mx":"0",
                    "enabled":"1",
                    "status":"enabled",
                    "monitor_status":"",
                    "remark":"",
                    "updated_on":"2012-11-16 15:52:56",
                    "hold":"hold"
                },
                {
                    "id":"16662142",
                    "name":"@",
                    "line":"\u9ed8\u8ba4",
                    "type":"NS",
                    "ttl":"600",
                    "value":"ns2.dnsv2.com.",
                    "mx":"0",
                    "enabled":"1",
                    "status":"enabled",
                    "monitor_status":"",
                    "remark":"",
                    "updated_on":"2012-11-16 15:52:56",
                    "hold":"hold"
                }
            ]
        }

修改记录
---------
接口地址：
    *  https://dnsapi.cn/Record.Modify
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 域名ID，必选
    * record_id 记录ID，必选
    * sub_domain 主机记录，默认@，如 www，可选
    * record_type 记录类型，通过API记录类型获得，大写英文，比如：A，必选
    * record_line 记录线路，通过API记录线路获得，中文，比如：默认，必选
    * value 记录值, 如 IP:200.200.200.200, CNAME: cname.dnspod.com., MX: mail.dnspod.com.，必选
    * mx {1-20} MX优先级, 当记录类型是 MX 时有效，范围1-20, mx记录必选
    * ttl {1-604800} TTL，范围1-604800，不同等级域名最小值不同，可选
响应代码：
    * 共通返回
    * -15 域名已被封禁
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 7 不是域名所有者或没有权限
    * 8 记录ID错误
    * 21 域名被锁定
    * 22 子域名不合法
    * 23 子域名级数超出限制
    * 24 泛解析子域名错误
    * 25 轮循记录数量超出限制
    * 26 记录线路错误
    * 27 记录类型错误
    * 29 TTL 值太小
    * 30 MX 值错误，1-20
    * 31 URL记录数超出限制
    * 32 NS 记录数超出限制
    * 33 AAAA 记录数超出限制
    * 34 记录值非法
    * 35 添加的IP不允许
    * 36 @主机的NS纪录只能添加默认线路

示例::

    curl -X POST https://dnsapi.cn/Record.Modify -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2317346&record_id=16894439&value=3.2.2.2&record_type=A&record_line=默认'
   
返回参考：

    * JSON::

        {
            "status": {
                "code":"1",
                "message":"Action completed successful",
                "created_at":"2012-11-24 16:53:23"
            },
            "record": {
                "id":16894439,
                "name":"@",
                "value":"3.2.2.2","status":"enable"
            }
        }

删除记录
---------
接口地址：
    *  https://dnsapi.cn/Record.Remove
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 域名ID，必选
    * record_id 记录ID，必选
响应代码：
    * 共通返回
    * -15 域名已被封禁
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 7 不是域名所有者或没有权限
    * 8 记录ID错误
    * 21 域名被锁定

示例::

    curl -X POST https://dnsapi.cn/Record.Remove -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2317346&record_id=16894439'
    
返回参考：

    * JSON::

        {
            "status": {
                "code":"1",
                "message":"Action completed successful",
                "created_at":"2012-11-24 16:58:07"
            }
        }

更新动态DNS记录
----------------
接口地址：
    *  https://dnsapi.cn/Record.Ddns
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 域名ID，必选
    * record_id 记录ID，必选
    * record_line 记录线路，通过API记录线路获得，中文，比如：默认，必选
响应代码：
    * 共通返回
    * -15 域名已被封禁
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 7 不是域名所有者或没有权限
    * 8 记录ID错误
    * 21 域名被锁定
    * 22 子域名不合法
    * 23 子域名级数超出限制，比如免费套餐域名不支持三级域名
    * 24 泛解析子域名错误，比如免费套餐载名不支持 a*
    * 25 轮循记录数量超出限制，比如免费套餐域名只能存在两条轮循记录
    * 26 记录线路错误，比如免费套餐域名不支持移动、国外

示例::

    curl -X POST https://dnsapi.cn/Record.Ddns -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2317346&record_id=16894439&record_line=默认'
    
返回参考：

    * JSON::

        { 
            "status": {
                "code":"1",
                "message":"Action completed successful",
                "created_at":"2012-11-24 17:23:58"
            },
            "record": {
                "id":16909160,
                "name":"@",
                "value":"111.111.111.111"
            }
        }

设置记录备注
-------------
接口地址：
    *  https://dnsapi.cn/Record.Remark
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 域名ID，必选
    * record_id 记录ID，必选
    * remark 域名备注，删除备注请提交空内容，必选
响应代码：
    * 共通返回
    * 6 域名ID错误
    * 8 记录 ID 错误

示例::

    curl -X POST https://dnsapi.cn/Record.Remark -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2317346&record_id=16894439&remark=test'
    
返回参考：

    * JSON::

        {
            "status": {
                "code": "1", 
                "message": "Action completed successful", 
                "created_at": "2012-11-24 17:32:23"
            }
        }


获取记录信息
-------------
接口地址：
    *  https://dnsapi.cn/Record.Info
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 域名ID，必选
    * record_id 记录ID，必选
响应代码：
    * 共通返回
    * -15 域名已被封禁
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 7 不是域名所有者或没有权限
    * 8 记录ID错误

示例::

    curl -X POST https://dnsapi.cn/Record.Info -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2317346&record_id=16894439'
    
返回参考：

    * JSON::

        {
            "status": {
                "code": "1", 
                "message": "Action completed successful", 
                "created_at": "2012-11-24 17:36:10"
            }, 
            "domain": {
                "id": 2317346, 
                "domain": "testapi.com", 
                "domain_grade": "D_Plus"
            }, 
            "record": {
                "id": "16909160", 
                "sub_domain": "@", 
                "record_type": "A", 
                "record_line": "默认", 
                "value": "111.111.111.111", 
                "mx": "0", 
                "ttl": "10", 
                "enabled": "1", 
                "monitor_status": "", 
                "remark": "test", 
                "updated_on": "2012-11-24 17:23:58", 
                "domain_id": "2317346"
            }
        }


设置记录状态
-------------
接口地址：
    *  https://dnsapi.cn/Record.Status
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 域名ID，必选
    * record_id 记录ID，必选
    * status {enabled|disabled} 新的状态，必选
响应代码：
    * 共通返回
    * -15 域名已被封禁
    * -7 企业账号的域名需要升级才能设置
    * -8 代理名下用户的域名需要升级才能设置
    * 6 域名ID错误
    * 7 不是域名所有者或没有权限
    * 8 记录ID错误
    * 21 域名被锁定

示例::

    curl -X POST https://dnsapi.cn/Record.Info -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2317346&record_id=16894439&status=disable'
    
返回参考：

    * JSON::

        {
            "status": {
                "code": "1", 
                "message": "Action completed successful", 
                "created_at": "2012-11-24 20:07:29"
            }, 
            "record": {
                "id": 16909160, 
                "name": "@", 
                "status": "disable"
            }
        }


宕机监控
=========
列出包含A记录的子域名
-----------------------
接口地址：
    *  https://dnsapi.cn/Monitor.Listsubdomain
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain或domain_id, 必选
响应代码：
    * 共通返回
    * 6 域名不存在
    * 7 域名编号错误
    * 8 此域名没有任何记录

示例::

    curl -X POST https://dnsapi.cn/Monitor.Listsubdomain -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2317346'
    
返回参考：

    * JSON::

        {
            "status": {
                "code": "1", 
                "message": "Action completed successful", 
                "created_at": "2012-11-24 20:14:31"
            }, 
            "domain": {
                "id": 2317346, 
                "name": "testapi.com", 
                "punycode": "testapi.com", 
                "grade": "D_Plus", 
                "owner": "api@dnspod.com"
            }, 
            "subdomain": [
                "@"
            ]
        }

列出子域名的A记录
------------------
接口地址：
    *  https://dnsapi.cn/Monitor.Listsubvalue
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain或domain_id, 必选
    * subdomain 子域名，必选
响应代码：
    * 共通返回
    * 6 域名不存在
    * 7 域名编号错误

示例::

    curl -X POST https://dnsapi.cn/Monitor.Listsubvalue -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2317346&subdomain=@'
    
返回参考：

    * JSON::

        {
            "status": {
                "code": "1", 
                "message": "Action completed successful", 
                "created_at": "2012-11-24 20:19:05"
            }, 
            "domain": {
                "id": 2317346, 
                "name": "testapi.com", 
                "punycode": "testapi.com", 
                "grade": "D_Plus"
            }, 
            "points": {
                "max": 999, 
                "list": {
                    "ctc": "电信", 
                    "cuc": "联通", 
                    "cmc": "移动"
                }
            }, 
            "records": [
                {
                    "id": "16909160", 
                    "area": "默认", 
                    "value": "119.180.24.194"
                }
            ]
        }

监控列表
---------
接口地址：
    *  https://dnsapi.cn/Monitor.List
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
响应代码：
    * 共通返回

示例::

    curl -X POST https://dnsapi.cn/Monitor.List -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2317346'
    
返回参考：

    * JSON::

        {
            "status": {
                "code": "1", 
                "message": "Action completed successful", 
                "created_at": "2012-11-24 20:48:02"
            }, 
            "info": {
                "total_count": 1, 
                "down_count": 0
            }, 
            "monitors": [
                {
                    "monitor_id": "281ecb9e-3635-11e2-bab7-0819a6248970", 
                    "domain": "testapi.com", 
                    "domain_id": "2317346", 
                    "domain_grade": "D_Plus", 
                    "record_id": "16909160", 
                    "sub_domain": "@", 
                    "record_line": "默认", 
                    "ip": "119.180.24.194", 
                    "now_ip": "119.180.24.194", 
                    "host": "testapi.com", 
                    "port": "80", 
                    "monitor_type": "http", 
                    "monitor_path": "/", 
                    "monitor_interval": "360", 
                    "points": "ctc,cuc,cmc", 
                    "bak_ip": "auto", 
                    "status": "Ok", 
                    "status_code": "200", 
                    "sms_notice": "me", 
                    "email_notice": "me", 
                    "less_notice": "yes", 
                    "callback_url": "", 
                    "callback_key": "", 
                    "monitor_status": "enabled", 
                    "created_on": "2012-11-24 20:47:51", 
                    "updated_on": "2012-11-24 20:47:51", 
                    "bak_ip_status": [ ]
                }
            ]
        }

监控添加
---------
接口地址：
    *  https://dnsapi.cn/Monitor.Create
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 域名编号，必选
    * record_id 记录编号，必选
    * port 监控端口，比如80，必选
    * monitor_interval 监控间隔，支持{60|180|360|}，必选
    * host 监控主机头，比如 www.dnspod.cn，必选
    * monitor_type 监控类型，支持{http|https}，必选
    * monitor_path 监控路径，比如/，必选
    * points 监控节点，用,分隔多个，只能选择列表中的节点，并且有数量限制，必选
    * bak_ip 宕机备用，必选，支持任选以下选项中的一个：
        #. pass 只监控，不切换
        #. pause 智能暂停
        #. auto 智能切换
        #. 用逗号分隔的IP 设置备用IP

    * keep_ttl 宕机不修改ttl，可选
    * sms_notice 短信通知，me域名所有者，share共享用户，用,分隔多选择，比如me,share, 可选
    * email_notice 邮件通知，me域名所有者，share共享用户，用,分隔多选择，比如me,share，可选
    * less_notice {yes|no}是否一个小时内只发一次通知，可选
    * callback_url  可选，回调URL，宕机了会将相关的参数提交到此设置的URL，具体参考回调URL说明，可选
    * callback_key 可选，回调密钥，如果设置了回调URL请设置此参数以保证安全，可选
响应代码：
    * 共通返回
    * 6 域名编号错误
    * 7 记录编号错误
    * 8 监控主机头错误
    * 9 监控端口错误，端口只能是正整数1~65535
    * 10 监控类型不正确
    * 11 监控路径不正确
    * 12  监控间隔不正确
    * 13 监控节点不正确
    * 14 监控节点数量超出限制
    * 15 备用IP不正确
    * 16 备用url不正确
    * 17 备用IP不正确
    * 18 短信设置不正确
    * 19 邮件设置不正确
    * 20 此记录已经存在监控
    * 21 监控数量超出限制
    * 22 回调URL不正确

示例::

    curl -X POST https://dnsapi.cn/Monitor.Create -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2317346&record_id=16909160&port=80&monitor_type=http&monitor_path=/&monitor_interval=360&points=ctc,cuc,cmc&bak_ip=pass&host=testapi.com'

返回参考：

    * JSON::

        {
            "status": {
                "code": "1", 
                "message": "Action completed successful", 
                "created_at": "2012-11-24 21:25:30"
            }, 
            "monitor": {
                "monitor_id": "6aac176e-363a-11e2-bab7-0819a6248970", 
                "record_id": 16909160
            }
        }

监控修改
---------
接口地址：
    *  https://dnsapi.cn/Monitor.Modify
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * monitor_id 监控编号，必选
    * port 监控端口，比如80，必选
    * monitor_interval 监控间隔，支持{60|180|360|}，必选
    * monitor_type 监控类型，支持{http|https}，必选
    * monitor_path 监控路径，比如/，必选
    * points 监控节点，用,分隔多个，只能选择列表中的节点，并且有数量限制，必选
    * bak_ip 宕机备用，必选，支持任选以下选项中的一个：
        #. pass 只监控，不切换
        #. pause 智能暂停
        #. auto 智能切换
        #. 用逗号分隔的IP 设置备用IP

    * host 监控主机头，比如 www.dnspod.cn，可选
    * keep_ttl 宕机不修改ttl，可选
    * sms_notice 短信通知，me域名所有者，share共享用户，用,分隔多选择，比如me,share, 可选
    * email_notice 邮件通知，me域名所有者，share共享用户，用,分隔多选择，比如me,share，可选
    * less_notice {yes|no}是否一个小时内只发一次通知，可选
    * callback_url  可选，回调URL，宕机了会将相关的参数提交到此设置的URL，具体参考回调URL说明，可选
    * callback_key 可选，回调密钥，如果设置了回调URL请设置此参数以保证安全，可选
响应代码：
    * 共通返回
    * 7 监控编号错误
    * 8 监控主机头错误
    * 9 监控端口错误，端口只能是正整数1~65535
    * 10 监控类型不正确
    * 11 监控路径不正确
    * 12  监控间隔不正确
    * 13 监控节点不正确
    * 14 监控节点数量超出限制
    * 15 备用IP不正确
    * 16 备用url不正确
    * 17 备用IP不正确
    * 18 短信设置不正确
    * 19 邮件设置不正确
    * 22 回调URL不正确

示例::

    curl -X POST https://dnsapi.cn/Monitor.Modify -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2317346&monitor_id=51fc9a20-363c-11e2-bab7-0819a6248970&port=80&monitor_type=http&monitor_path=/&monitor_interval=360&points=ctc,cuc,cmc&bak_ip=pass'

返回参考：

    * JSON::

        {
            "status": {
                "code": "1", 
                "message": "Action completed successful", 
                "created_at": "2012-11-24 21:41:31"
            }
        }

监控删除
---------
接口地址：
    *  https://dnsapi.cn/Monitor.Remove
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * monitor_id 监控编号
响应代码：
    * 共通返回
    * 6 监控编号错误

示例::

    curl -X POST https://dnsapi.cn/Monitor.Modify -d 'login_email=api@dnspod.com&login_password=password&format=json&monitor_id=51fc9a20-363c-11e2-bab7-0819a6248970'
    
返回参考：

    * JSON::

        {
            "status": {
                "code": "1", 
                "message": "Action completed successful", 
                "created_at": "2012-11-24 21:51:49"
            }
        }


获取监控信息
-------------
接口地址：
    * https://dnsapi.cn/Monitor.Info
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * monitor_id 监控编号
响应代码：
    * 共通返回
    * 7 监控编号错误

示例::
        
    curl -X POST https://dnsapi.cn/Monitor.Info -d 'login_email=api@dnspod.com&login_password=password&format=json&monitor_id=e91997aa-3641-11e2-bab7-0819a6248970'
    
返回参考：

    * JSON::

        {
            "status": {
                "code": "1", 
                "message": "Action completed successful", 
                "created_at": "2012-11-24 22:19:58"
            }, 
            "info": {
                "monitor_id": "e91997aa-3641-11e2-bab7-0819a6248970", 
                "domain": "testapi.com", 
                "domain_id": "2317346", 
                "domain_grade": "D_Plus", 
                "record_id": "16909160", 
                "sub_domain": "@", 
                "record_line": "默认", 
                "ip": "119.180.24.194", 
                "now_ip": "119.180.24.194", 
                "host": "testapi.com", 
                "port": "80", 
                "monitor_type": "http", 
                "monitor_path": "/", 
                "monitor_interval": "180", 
                "points": "ctc,cuc,cmc", 
                "bak_ip": "pass", 
                "status": "Ok", 
                "status_code": "200", 
                "sms_notice": "me", 
                "email_notice": "me", 
                "less_notice": "no", 
                "callback_url": "", 
                "callback_key": "", 
                "monitor_status": "enabled", 
                "created_on": "2012-11-24 22:19:09", 
                "updated_on": "2012-11-24 22:19:09", 
                "bak_ip_status": [ ]
            }
        }

设置监控状态
-------------
接口地址：
    *  https://dnsapi.cn/Monitor.Setstatus
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * monitor_id 监控编号，必选
    * status {enabled|disabled} 新的状态，必选
响应代码：
    * 共通返回
    * 6 监控编号错误
    * 7 新状态代码错误
    * 8 请先启用域名
    * 9 请先启用记录
响应代码：
    * 共通返回
    * 6 监控编号错误

示例::

    curl -X POST https://dnsapi.cn/Monitor.Setstatus -d 'login_email=api@dnspod.com&login_password=password&format=json&monitor_id=03e3b268-3643-11e2-bab7-0819a6248970&status=disable'
    
返回参考：

    * JSON::

        {
            "status": {
                "code": "1", 
                "message": "Action completed successful", 
                "created_at": "2012-11-24 22:33:20"
            }
        }

获取监控历史
-------------
接口地址：
    *  https://dnsapi.cn/Monitor.Gethistory
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * monitor_id 监控编号，必选
    * hours 获取最近多少个小时的记录，可选
响应代码：
    * 共通返回
    * 6 监控编号错误

示例::

    curl -X POST https://dnsapi.cn/Monitor.Setstatus -d 'login_email=api@dnspod.com&login_password=password&format=json&monitor_id=03e3b268-3643-11e2-bab7-0819a6248970&hours=1'
    
返回参考：

    * JSON::

        {
            "status": {
                "code": "1", 
                "message": "Action completed successful", 
                "created_at": "2012-11-24 22:40:02"
            }, 
            "domain": {
                "id": "2317346", 
                "domain": "testapi.com", 
                "domain_grade": "D_Plus"
            }, 
            "record": {
                "id": "16909160", 
                "sub_domain": "@", 
                "ip": "119.180.24.194"
            }, 
            "monitor_history": [
                {
                    "data": {
                        "message": "ok", 
                        "code": 200, 
                        "data": [
                            {
                                "status": "Down", 
                                "status_code": -3, 
                                "createtime": "2012-11-24 22:28:31", 
                                "responsetime": 0
                            }, 
                            {
                                "status": "Down", 
                                "status_code": -3, 
                                "createtime": "2012-11-24 22:31:31", 
                                "responsetime": 0
                            }, 
                            {
                                "status": "Down", 
                                "status_code": -3, 
                                "createtime": "2012-11-24 22:34:31", 
                                "responsetime": 999
                            }, 
                            {
                                "status": "Down", 
                                "status_code": -3, 
                                "createtime": "2012-11-24 22:37:31", 
                                "responsetime": 1
                            }
                        ]
                    }, 
                    "point": "ctc"
                }, 
                {
                    "data": {
                        "message": "ok", 
                        "code": 200, 
                        "data": [
                            {
                                "status": "Down", 
                                "status_code": -3, 
                                "createtime": "2012-11-24 22:28:52", 
                                "responsetime": 0
                            }, 
                            {
                                "status": "Down", 
                                "status_code": -3, 
                                "createtime": "2012-11-24 22:31:52", 
                                "responsetime": 0
                            }, 
                            {
                                "status": "Down", 
                                "status_code": -3, 
                                "createtime": "2012-11-24 22:34:52", 
                                "responsetime": 0
                            }, 
                            {
                                "status": "Down", 
                                "status_code": -3, 
                                "createtime": "2012-11-24 22:37:52", 
                                "responsetime": 0
                            }
                        ]
                    }, 
                    "point": "cuc"
                }, 
                {
                    "data": {
                        "message": "ok", 
                        "code": 200, 
                        "data": [
                            {
                                "status": "Down", 
                                "status_code": -3, 
                                "createtime": "2012-11-24 22:30:07", 
                                "responsetime": 1
                            }, 
                            {
                                "status": "Down", 
                                "status_code": -3, 
                                "createtime": "2012-11-24 22:33:05", 
                                "responsetime": 0
                            }, 
                            {
                                "status": "Down", 
                                "status_code": -3, 
                                "createtime": "2012-11-24 22:36:06", 
                                "responsetime": 1
                            }, 
                            {
                                "status": "Down", 
                                "status_code": -3, 
                                "createtime": "2012-11-24 22:39:06", 
                                "responsetime": 1
                            }
                        ]
                    }, 
                    "point": "cmc"
                }
            ]
        }

获取监控概况
-------------
接口地址：
    * https://dnsapi.cn/Monitor.Userdesc
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
响应代码：
    * 共通返回

示例::

    curl -X POST https://dnsapi.cn/Monitor.Userdesc -d 'login_email=api@dnspod.com&login_password=password&format=json'
    
返回参考：

    * JSON::

        {
            "status": {
                "code": "1", 
                "message": "Action completed successful", 
                "created_at": "2012-11-24 22:50:14"
            }, 
            "desc": {
                "unmoniting_count": 3, 
                "moniting_count": 1, 
                "down_count": 1
            }, 
            "user": {
                "max_count": 28, 
                "use_count": 1
            }
        }


获取监控警告
-------------
接口地址：
    *  https://dnsapi.cn/Monitor.Getdowns
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * offset 记录开始的偏移，第一条记录为 0，依次类推，可选
    * length 共要获取的记录的数量，比如获取20条，则为20，可选
响应代码：
    * 共通返回

示例::

    curl -X POST https://dnsapi.cn/Monitor.Getdowns -d 'login_email=api@dnspod.com&login_password=password&format=json&offset=0&length=10'
    
返回参考：

    * JSON::

        {
            "status": {
                "code": "1", 
                "message": "Action completed successful", 
                "created_at": "2012-11-24 22:54:03"
            }, 
            "info": {
                "total_count": "1"
            }, 
            "monitor_downs": [
                {
                    "monitor_id": "03e3b268-3643-11e2-bab7-0819a6248970", 
                    "host": "testapi.com", 
                    "record_line": "默认", 
                    "ip": "119.180.24.194", 
                    "warn_reason": "连接超时|访问您主机时连接超时，并且重试了5次后依然超时，建议您检查下你的服务器是否有网络不稳定的情况移动:timed out网通:timed out电信:timed out", 
                    "switch_log": [ ], 
                    "created_on": "2012-11-24 22:30:06", 
                    "updated_on": "0000-00-00 00:00:00"
                }
            ]
        }
