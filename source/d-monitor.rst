D监控相关
=========

.. _Monitor.Listsubdomain:

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

.. _Monitor.Listsubvalue:

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

.. _Monitor.List:

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

.. _Monitor.Create:

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
        #. pause 老版智能暂停功能，详见https://support.dnspod.cn/Kb/showarticle/tsid/179
        #. pause2 智能暂停v2, 发现ip宕机后直接暂停该记录，无其它规则
        #. auto 智能切换
        #. 用逗号分隔的IP 设置备用IP

    * keep_ttl {yes|no} 宕机切换后是否修改ttl，可选，默认为 no
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

.. _Monitor.Modify:

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
        #. pause 老版智能暂停功能，详见https://support.dnspod.cn/Kb/showarticle/tsid/179
        #. pause2 智能暂停v2, 发现ip宕机后直接暂停该记录，无其它规则
        #. auto 智能切换
        #. 用逗号分隔的IP 设置备用IP

    * host 监控主机头，比如 www.dnspod.cn，可选
    * keep_ttl {yes|no} 宕机切换后是否修改ttl，可选，默认为 no
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

.. _Monitor.Remove:

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

.. _Monitor.Info:

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

.. _Monitor.Setstatus:

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

.. _Monitor.Gethistory:

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

.. _Monitor.Userdesc:

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

.. _Monitor.Getdowns:

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
