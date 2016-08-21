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
    * domain_id 或 domain, 分别对应域名ID和域名, 提交其中一个即可
    * sub_domain  主机记录, 如 www，可选，如果不传，默认为 @
    * record_type  记录类型，通过API记录类型获得，大写英文，比如：A, 必选
    * record_line  记录线路，通过API记录线路获得，中文，比如：默认
    * record_line_id 线路的ID，通过API记录线路获得，英文字符串，比如：'10=1' 【record_line 和 record_line_id 二者传其一即可，系统优先取 record_line_id】
    * value  记录值, 如 IP:200.200.200.200, CNAME: cname.dnspod.com., MX: mail.dnspod.com., 必选
    * mx {1-20}  MX优先级, 当记录类型是 MX 时有效，范围1-20, MX记录必选
    * ttl {1-604800}  TTL，范围1-604800，不同等级域名最小值不同, 可选
    * status ["enable", "disable"]，记录初始状态，默认为"enable"，如果传入"disable"，解析不会生效，也不会验证负载均衡的限制，可选
    * weight 权重信息，0到100的整数，可选。仅企业 VIP 域名可用，0 表示关闭，留空或者不传该参数，表示不设置权重信息
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
    * 500025 A记录负载均衡超出限制
    * 500026 CNAME记录负载均衡超出限制
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

    curl -X POST https://dnsapi.cn/Record.Create -d 'login_token=LOGIN_TOKEN&format=json&domain_id=2317346&sub_domain=@&record_type=A&record_line_id=10%3D3&value=1.1.1.1'
    
返回参考：

    * JSON::

        {
            "status": {
                "code":"1",
                "message":"Action completed successful",
                "created_at":"2015-01-19 22:17:47"
            },
            "record": {
                "id":"16894439",
                "name":"@",
                "status":"enable"
            }
        }

    * 字段说明:
        * id: 记录ID, 即为 record_id
        * name: 添加的字域名 
        * status: 域名记录的状态

注意：
    record_line_id 形如 "10=3"，其中可能会包含等号，即 "="，如果是通过类似 URL 传递参数，需要将 '=' 转义成 '%3D'


.. _Record.List:

记录列表
---------
接口地址：
    * https://dnsapi.cn/Record.List
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain, 分别对应域名ID和域名, 提交其中一个即可
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

     curl -X POST https://dnsapi.cn/Record.List -d 'login_token=LOGIN_TOKEN&format=json&domain_id=2317346'
    
返回参考：

    * JSON::

            {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2016-08-12 17:57:55"
            },
            "domain": {
                "id": 9842292,
                "name": "yizerowu.com",
                "punycode": "yizerowu.com",
                "grade": "DP_Free",
                "owner": "yizerowu@tencent.com",
                "ext_status": "dnserror",
                "ttl": 600
            },
            "info": {
                "sub_domains": "12",
                "record_total": "2"
            },
            "records": [
                {
                    "id": "44146112",
                    "name": "yizerowwwww",
                    "line": "联通",
                    "line_id": "10=1",
                    "type": "A",
                    "ttl": "600",
                    "value": "12.3.3.3",
                    "weight": null,
                    "mx": "0",
                    "enabled": "1",
                    "status": "enabled",
                    "monitor_status": "",
                    "remark": "",
                    "updated_on": "2016-08-12 17:07:05",
                    "use_aqb": "no"
                },
                {
                    "id": "44146111",
                    "name": "yizerowwwww",
                    "line": "教育网",
                    "line_id": "10=2",
                    "type": "A",
                    "ttl": "600",
                    "value": "12.3.3.3",
                    "weight": null,
                    "mx": "0",
                    "enabled": "1",
                    "status": "enabled",
                    "monitor_status": "",
                    "remark": "",
                    "updated_on": "2016-08-12 17:00:19",
                    "use_aqb": "no"
                }
            ]
        }



    * 字段说明:
        * domain:
            * id: 域名ID，即为 domain_id
            * name: 域名
            * punycode: punycode 转码之后的域名
            * grade: 域名等级，详见 Domain.List 或 Domain.Info 接口
            * owner: 域名所有者
        * info:
            * sub_domains: 域名记录条数
            * record_total: 域名记录条数
        * records:
            * id: 记录ID编号
            * name: 子域名(主机记录)
            * line: 解析记录的线路, 详见 Record.Line 接口
            * line_id: 解析记录的线路ID，详见 Record.Line 接口
            * type: 记录类型, 详见 Record.Type 接口
            * ttl: 记录的 TTL 值
            * value: 记录值
            * mx: 记录的 MX 记录值, 非 MX 记录类型，默认为 0
            * enabled: 记录状态
                * "0": 禁用
                * "1": 启用
            * status: 系统内部标识状态, 开发者可忽略
            * monitor_status: 该记录的D监控状态
                * "Ok": 服务器正常
                * "Warn": 该记录有报警, 服务器返回 4XX
                * "Down": 服务器宕机
                * "": 该记录未开启D监控
            * remark: 记录备注
            * updated_on: 记录最后更新时间
            * use_aqb: 是否开通网站安全中心
                * "yes": 已经开启
                * "no": 未开启


.. _Record.Modify:

修改记录
---------
接口地址：
    *  https://dnsapi.cn/Record.Modify
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain, 分别对应域名ID和域名, 提交其中一个即可
    * record_id 记录ID，必选
    * sub_domain  主机记录, 如 www，可选，如果不传，默认为 @
    * record_type 记录类型，通过API记录类型获得，大写英文，比如：A，必选
    * record_line 记录线路，通过API记录线路获得，中文，比如：默认，必选
    * record_line_id 线路的ID，通过API记录线路获得，英文字符串，比如：'10=1' 【record_line 和 record_line_id 二者传其一即可，系统优先取 record_line_id】
    * value 记录值, 如 IP:200.200.200.200, CNAME: cname.dnspod.com., MX: mail.dnspod.com.，必选
    * mx {1-20} MX优先级, 当记录类型是 MX 时有效，范围1-20, mx记录必选
    * ttl {1-604800} TTL，范围1-604800，不同等级域名最小值不同，可选
    * status ["enable", "disable"]，记录状态，默认为"enable"，如果传入"disable"，解析不会生效，也不会验证负载均衡的限制，可选
    * weight 权重信息，0到100的整数，可选。仅企业 VIP 域名可用，0 表示关闭，留空或者不传该参数，表示不设置权重信息
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
    * 500025 A记录负载均衡超出限制
    * 500026 CNAME记录负载均衡超出限制
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

    curl -X POST https://dnsapi.cn/Record.Modify -d 'login_token=LOGIN_TOKEN&format=json&domain_id=2317346&record_id=16894439&sub_domain=www&value=3.2.2.2&record_type=A&record_line_id=10%3D3'
   
返回参考：

    * JSON::

        {
            "status": {
                "code":"1",
                "message":"Action completed successful",
                "created_at":"2015-01-18 16:53:23"
            },
            "record": {
                "id":16894439,
                "name":"@",
                "value":"3.2.2.2",
                "status":"enable"
            }
        }

    * 字段说明:
        * id: 记录ID, 即为 record_id
        * name: 子域名
        * value": 记录值
        * status": 记录状态

注意：
    record_line_id 形如 "10=3"，其中可能会包含等号，即 "="，如果是通过类似 URL 传递参数，需要将 '=' 转义成 '%3D'


.. _Record.Remove:

删除记录
---------
接口地址：
    *  https://dnsapi.cn/Record.Remove
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain, 分别对应域名ID和域名, 提交其中一个即可
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

    curl -X POST https://dnsapi.cn/Record.Remove -d 'login_token=LOGIN_TOKEN&format=json&domain_id=2317346&record_id=16894439'
    
返回参考：

    * JSON::

        {
            "status": {
                "code":"1",
                "message":"Action completed successful",
                "created_at":"2015-01-18 16:58:07"
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
    * domain_id 或 domain, 分别对应域名ID和域名, 提交其中一个即可
    * record_id 记录ID，必选
    * sub_domain 主机记录，如 www
    * record_line 记录线路，通过API记录线路获得，中文，比如：默认，必选
    * record_line_id 线路的ID，通过API记录线路获得，英文字符串，比如：'10=1' 【record_line 和 record_line_id 二者传其一即可，系统优先取 record_line_id】
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

    curl -X POST https://dnsapi.cn/Record.Ddns -d 'login_token=LOGIN_TOKEN&format=json&domain_id=2317346&record_id=16894439&record_line_id=10%3D3&sub_domain=www'
    
返回参考：

    * JSON::

        { 
            "status": {
                "code":"1",
                "message":"Action completed successful",
                "created_at":"2015-01-18 17:23:58"
            },
            "record": {
                "id":16909160,
                "name":"@",
                "value":"111.111.111.111"
            }
        }

    * 字段说明:
        * id: 记录ID, 即为 record_id
        * name: 子域名
        * value": 记录值

注意：
    record_line_id 形如 "10=3"，其中可能会包含等号，即 "="，如果是通过类似 URL 传递参数，需要将 '=' 转义成 '%3D'


.. _Record.Remark:


设置记录备注
-------------
接口地址：
    *  https://dnsapi.cn/Record.Remark
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain, 分别对应域名ID和域名, 提交其中一个即可
    * record_id 记录ID，必选
    * remark 域名备注，删除备注请提交空内容，必选
响应代码：
    * 共通返回
    * 6 域名ID错误
    * 8 记录 ID 错误

示例::

    curl -X POST https://dnsapi.cn/Record.Remark -d 'login_token=LOGIN_TOKEN&format=json&domain_id=2317346&record_id=16894439&remark=test'
    
返回参考：

    * JSON::

        {
            "status": {
                "code": "1", 
                "message": "Action completed successful", 
                "created_at": "2015-01-18 17:32:23"
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
    * domain_id 或 domain, 分别对应域名ID和域名, 提交其中一个即可
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

    curl -X POST https://dnsapi.cn/Record.Info -d 'login_token=LOGIN_TOKEN&format=json&domain_id=2317346&record_id=16894439'
    
返回参考：

    * JSON::

        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2016-08-12 17:51:25"
            },
            "domain": {
                "id": 9842292,
                "domain": "yizerowu.com",
                "domain_grade": "DP_Free"
            },
            "record": {
                "id": "44146112",
                "sub_domain": "yizerowwwww",
                "record_type": "A",
                "record_line": "联通",
                "record_line_id": "10=1",
                "value": "12.3.3.3",
                "weight": null,
                "mx": "0",
                "ttl": "600",
                "enabled": "1",
                "monitor_status": "",
                "remark": "",
                "updated_on": "2016-08-12 17:07:05",
                "domain_id": "9842292"
            }
        }


    * 字段说明:
        * domain:
            * id: 域名ID，即为 domain_id
            * domain: 域名
            * domain_grade: 域名等级，详见 Domain.List 或 Domain.Info 接口
        * record:
            * id: 记录ID编号
            * sub_domain: 子域名(主机记录)
            * record_type: 记录类型, 详见 Record.Type 接口
            * record_line: 解析记录的线路, 详见 Record.Line 接口
            * record_line_id: 解析记录的线路ID，详见 Record.Line 接口
            * value: 记录值
            * mx: 记录的 MX 记录值, 非 MX 记录类型，默认为 0
            * ttl: 记录的 TTL 值
            * enabled: 记录状态
                * "0": 禁用
                * "1": 启用
            * monitor_status: 该记录的D监控状态
                * "Ok": 服务器正常
                * "Warn": 该记录有报警, 服务器返回 4XX
                * "Down": 服务器宕机
                * "": 该记录未开启D监控
            * remark: 记录备注
            * updated_on: 记录最后更新时间
            * domain_id: 域名ID, 即为 domain_id

.. _Record.Status:

设置记录状态
-------------
接口地址：
    *  https://dnsapi.cn/Record.Status
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 或 domain, 分别对应域名ID和域名, 提交其中一个即可
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

    curl -X POST https://dnsapi.cn/Record.Status -d 'login_token=LOGIN_TOKEN&format=json&domain_id=2317346&record_id=16894439&status=disable'
    
返回参考：

    * JSON::

        {
            "status": {
                "code": "1", 
                "message": "Action completed successful", 
                "created_at": "2015-01-18 20:07:29"
            }, 
            "record": {
                "id": 16909160, 
                "name": "@", 
                "status": "disable"
            }
        }

    * 字段说明:
        * id: 记录ID, 即为 record_id
        * name: 子域名
        * status: 记录状态
