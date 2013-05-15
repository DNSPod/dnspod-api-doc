帐户相关
========


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

