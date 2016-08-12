帐户相关
========

.. _User.Detail:

获取帐户信息
------------
接口地址：
    * https://dnsapi.cn/User.Detail
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
响应代码：
    * 共通返回

示例::
    
    curl -X POST https://dnsapi.cn/User.Detail -d 'login_token=LOGIN_TOKEN&format=json'

返回参考：

    * JSON::

        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2015-01-18 15:04:07"
            },
            "info": {
                "user": {
                    "real_name": "",
                    "user_type": "personal",
                    "telephone": "18754553214",
                    "im": "10000000",
                    "nick": "DNSPod 先生",
                    "id": "625033",
                    "email": "api@dnspod.com",
                    "status": "enabled",
                    "email_verified": "no",
                    "telephone_verified": "no",
                    "weixin_binded": "no",
                    "agent_pending": false,
                    "balance": 0,
                    "smsbalance": 0,
                    "user_grade": "DP_Free"
                }
            }
        }        


    * 字段说明:
        ===================     ====================
        名称                    说明
        ===================     ====================
        real_name               用户名称, 企业用户对应为公司名称
        user_type               账号类型：["personal","enterprise"]，分别对应个人用户和企业用户
        telephone               电话号码
        im                      用户 IM (已废弃)
        nick                    用户昵称
        id                      用户 ID, 即为 user_id
        email                   用户账号, 邮箱格式
        status                  账号状态："enabled": 正常；"disabled": 被封禁
        email_verified          邮箱是否通过验证："yes": 通过；"no": 未通过
        telephone_verified      手机是否通过验证："yes": 通过；"no": 未通过
        weixin_binded           是否绑定微信："yes": 通过；"no": 未通过
        agent_pending           是否正在申请成为代理：true: 是；false: 否
        balance                 账号余额
        smsbalance              剩余短信条数
        user_grade              账号等级, 按照用户账号下域名等级排序, 选取一个最高等级为账号等级, 具体对应情况参见域名等级
        ===================     ====================

.. _User.Modify:

修改资料
--------
接口地址：
    * https://dnsapi.cn/User.Modify
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * real_name 真实姓名, 如果用户类型是企业, 则为公司名称
    * nick 用户称呼, 用于与用户联系时称呼用户
    * telephone 用户手机号码
    * im 用户qq
响应代码：
    * 共通返回
    * 8 手机号码不正确
    * 9 用户qq不正确

示例::
    
    curl -X POST https://dnsapi.cn/User.Modify -d 'login_token=LOGIN_TOKEN&format=json&im=10000000'

返回参考：

    * JSON::

        {
            "status": {
                "code":"1",
                "message":"Action completed successful",
                "created_at":"2015-01-18 13:34:56"
            }
        }

.. _Userpasswd.Modify:

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
    * 87 新密码不能和之前用过的密码一样
    * 88 新密码不安全, 请使用另外的密码

示例::
    
    curl -X POST https://dnsapi.cn/Userpasswd.Modify -d 'login_token=LOGIN_TOKEN&format=json&old_password=old_password&new_password=new_password'

返回参考：

    * JSON::

        {
            "status": {
                "code":"1",
                "message":"Action completed successful",
                "created_at":"2015-01-18 13:45:27"
            }
        }

.. _Useremail.Modify:

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

    curl -X POST https://dnsapi.cn/Useremail.Modify -d 'login_token=LOGIN_TOKEN&format=json&old_email=api1@dnspod.com&new_email=api@dnspod.com&password=password'

返回参考：

    * JSON::
        
        {
            "status": {
                "code":"1",
                "message":"Action completed successful",
                "created_at":"2015-01-18 14:49:41"
            }
        }

        
.. _User.Telephoneverify:

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
    
    curl -X POST https://dnsapi.cn/Telephoneverify.Code -d 'login_token=LOGIN_TOKEN&format=json&telephone=18600000000'

返回参考：

    * JSON::

        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2015-01-18 15:56:13"
            },
            "user": {
                "verify_code": "409752",
                "verify_desc": "请使用 18601234321 编辑短信，将 409752 发送至号码  159 6183 3568。"
            }
        }

    * 字段说明:
        * verify_code: 验证码
        * verify_desc: 描述文字


.. _User.Log:

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

    curl -X POST https://dnsapi.cn/User.Log -d 'login_token=LOGIN_TOKEN&format=json'

返回参考：

    * JSON::

        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2015-01-18 15:57:30"
            },
            "log": [
                "2015-01-18 15:12:02: 122.5.32.226 [山东省烟台市] 登陆 成功",
                "2015-01-15 23:13:25: (60.212.40.45) 添加域名 sssaavvvx.xyz (20690613)",
                "2015-01-10 09:38:50: 122.5.32.226 [山东省烟台市] 登陆 成功",
                "2015-01-04 11:26:24: 122.5.32.226 [山东省烟台市] 登陆 成功",
                "2014-12-23 15:39:36: (60.212.40.45) 添加域名 sssdadaa.com (20371179)",
                "2014-12-22 11:59:50: 60.212.40.45 [山东省烟台市] 登陆 成功",
                "2014-12-18 16:53:29: 122.5.32.226 [山东省烟台市] 登陆 成功",
                "2014-12-09 13:55:17: 60.212.40.45 [山东省烟台市] 登陆 成功",
                "2014-12-08 15:53:12: 122.5.32.226 [山东省烟台市] 登陆 成功",        
            ]
        }
