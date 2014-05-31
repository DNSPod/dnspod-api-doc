域名相关
========

.. _Domain.Create:

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

.. _Domain.List:

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

.. _Domain.Remove:

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

.. _Domain.Status:

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

.. _Domain.Info:

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

.. _Domain.Log:

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

.. _Domain.Searchenginepush:

设置搜索引擎推送
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

.. _Domainshare.Create:

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

.. _Domainshare.List:
    
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

.. _Domainshare.Modify:

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

.. _Domainshare.Remove:

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

.. _Domain.Transfer:

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

.. _Domain.Lock:

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

.. _Domain.Lockstatus:

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

.. _Domain.Unlock:

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

.. _Domainalias.List:

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


.. _Domainalias.Create:

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

.. _Domainalias.Remove:

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

.. _Domaingroup.List:

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
    * 该接口只对VIP帐户有效，免费用户无法获取域名分组，免费用户会返回错误提示。
    
.. _Domaingroup.Create:

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
    * 该接口只对VIP帐户有效，免费用户无法获取域名分组，免费用户会返回错误提示。

.. _Domaingroup.Modify:

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
    
.. _Domaingroup.Remove:

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
    
.. _Domain.Changegroup:

设置域名分组
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

.. _Domain.Ismark:

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

.. _Domain.Remark:

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

.. _Domain.Purview:

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

.. _Domain.Acquire:

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

.. _Domain.Acquiresend:

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

.. _Domain.Acquirevalidate:

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

.. _Record.Type:

获取等级允许的记录类型
----------------------------
接口地址：
    *  https://dnsapi.cn/Record.Type
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_grade 域名等级，分别为：D_Free, D_Plus, D_Extra, D_Expert, D_Ultra，分别对应免费套餐、个人豪华、企业1、企业2、企业3
                        新套餐：DP_Free DP_Plus DP_Extra DP_Expert DP_Ultra, 分别对应新免费、个人专业版、企业创业版、企业标准版、企业旗舰版
响应代码：
    * 共通返回
    * 6 等级代码不正确

示例::
    
    curl -X POST https://dnsapi.cn/Record.Type -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_grade=D_Free'

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

.. _Record.Line:

获取等级允许的线路线路
-----------------------
接口地址：
    *  https://dnsapi.cn/Record.Line
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_grade 域名等级，分别为：D_Free, D_Plus, D_Extra, D_Expert, D_Ultra，分别对应免费套餐、个人豪华、企业1、企业2、企业3。
                    新套餐：DP_Free, DP_Plus, DP_Extra, DP_Expert, DP_Ultra, 分别对应新免费、个人专业版、企业创业版、企业标准版、企业旗舰版
    * domain_id 或 domain，分别对应域名ID和域名，提交其中一个即可
响应代码：
    * 共通返回
    * 6 等级代码不正确

示例::
    
    curl -X POST https://dnsapi.cn/Record.Line -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_grade=D_Free&domain_id=2059079'

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

