记录相关
========

.. _Record.Create:

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
    * 31 存在冲突的记录(A记录、CNAME记录、URL记录不能共存)
    * 32 记录的TTL值超出了限制
    * 33 AAAA 记录数超出限制
    * 34 记录值非法
    * 36 @主机的NS纪录只能添加默认线路
    * 82 不能添加黑名单中的IP

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

.. _Record.List:

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
    * keyword，搜索的关键字，如果指定则只返回符合该关键字的记录，可选
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

.. _Record.Modify:

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
    * 82 不能添加黑名单中的IP

注意事项：
    * 如果1小时之内，提交了超过5次没有任何变动的记录修改请求，该记录会被系统锁定1小时，不允许再次修改。比如原记录值已经是 1.1.1.1，新的请求还要求修改为 1.1.1.1。

示例::

    curl -X POST https://dnsapi.cn/Record.Modify -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2317346&record_id=16894439&sub_domain=www&value=3.2.2.2&record_type=A&record_line=默认'
   
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

.. _Record.Remove:

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

.. _Record.Ddns:

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
    * sub_domain 主机记录，如 www
    * record_line 记录线路，通过API记录线路获得，中文，比如：默认，必选
    * value IP地址，例如：6.6.6.6，可选
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

注意事项：
    * 如果1小时之内，提交了超过5次没有任何变动的记录修改请求，该记录会被系统锁定1小时，不允许再次修改，所以在开发和测试的过程中，请自行处理IP变动，仅在本地IP发生变动的情况下才调用本接口。
    * 如何理解没有任何变动的记录修改请求？比如原记录值已经是 1.1.1.1，新的请求还要求修改为 1.1.1.1。

示例::

    curl -X POST https://dnsapi.cn/Record.Ddns -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2317346&record_id=16894439&record_line=默认&sub_domain=www'
    
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

.. _Record.Remark:

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

.. _Record.Info:

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

.. _Record.Status:

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
    * status {enable|disable} 新的状态，必选
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

    curl -X POST https://dnsapi.cn/Record.Status -d 'login_email=api@dnspod.com&login_password=password&format=json&domain_id=2317346&record_id=16894439&status=disable'
    
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
