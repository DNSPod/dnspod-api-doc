批量操作相关
============

.. _Batch.Domain.Create:

批量添加域名
------------
接口地址：
    * https://dnsapi.cn/Batch.Domain.Create
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domains 多个域名之间以英文的逗号分割
    * record_value 为每个域名添加 @ 和 www 的 A 记录值，记录值为IP，可选，如果不传此参数或者传空，将只添加域名，不添加记录
响应代码：
    * 共通返回【具体错误信息参见 message 字段】

示例::
    
    curl -X POST https://dnsapi.cn/Batch.Domain.Create -d 'login_token=LOGIN_TOKEN&format=json&domains=dnsapi1.cn,dnsapi2.cn,dnsapi3.cn,dnsapi4.cn&record_value=11.22.33.45'

返回参考：

    * JSON::

        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2015-09-08 16:28:11"
            },
            "job_id": "39581",
            "detail": [
                {
                    "id": 0,
                    "domain": "dnsapi1.cn",
                    "domain_grade": "DP_Free",
                    "err_msg": null,
                    "status": "waiting",
                    "operation": "plus",
                    "records": [
                        {
                            "id": 0,
                            "sub_domain": "www",
                            "record_type": "A",
                            "area": "默认",
                            "value": "11.22.33.45",
                            "ttl": 600,
                            "err_msg": null,
                            "status": "waiting",
                            "operation": "plus"
                        },
                        {
                            "id": 1,
                            "sub_domain": "@",
                            "record_type": "A",
                            "area": "默认",
                            "value": "11.22.33.45",
                            "ttl": 600,
                            "err_msg": null,
                            "status": "waiting",
                            "operation": "plus"
                        }
                    ]
                },
                {
                    "id": 1,
                    "domain": "dnsapi2.cn",
                    "domain_grade": "DP_Free",
                    "err_msg": null,
                    "status": "waiting",
                    "operation": "plus",
                    "records": [
                        {
                            "id": 0,
                            "sub_domain": "www",
                            "record_type": "A",
                            "area": "默认",
                            "value": "11.22.33.45",
                            "ttl": 600,
                            "err_msg": null,
                            "status": "waiting",
                            "operation": "plus"
                        },
                        {
                            "id": 1,
                            "sub_domain": "@",
                            "record_type": "A",
                            "area": "默认",
                            "value": "11.22.33.45",
                            "ttl": 600,
                            "err_msg": null,
                            "status": "waiting",
                            "operation": "plus"
                        }
                    ]
                }
            ]
        }

    * 字段说明:
        * job_id: 任务ID，批量操作为异步执行，拿到 job_id 说明批量操作已经成功加入到任务队列，可通过 Batch.Detail 接口异步去查询执行结果
        * detail: 任务详情，仅将用户的输入补全，并非实际执行结果，开发者可以忽略该字段


.. _Batch.Record.Create:

批量添加记录
------------
接口地址：
    * https://dnsapi.cn/Batch.Record.Create
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * domain_id 域名ID，多个 domain_id 用英文逗号进行分割
    * records 待批量添加的记录详情，JSON 字符串。形如：
        1. [{"sub_domain":"www,wap,bbs","record_type":"A","record_line":"默认","value":"11.22.33.44","ttl":600}]
        2. [{"sub_domain":"www,wap,bbs","record_type":"A","record_line":"默认","value":"11.22.33.44","ttl":600},{"sub_domain":"","record_type":"MX","record_line":"默认","value":"mx.qq.com","ttl":600,"MX":10}] 
响应代码：
    * 共通返回【具体错误信息参见 message 字段】

示例::
    
    curl -X POST https://dnsapi.cn/Batch.Record.Create -d 'login_token=LOGIN_TOKEN&format=json&records=[{"sub_domain":"www,wap,bbs","record_type":"A","record_line":"默认","value":"11.22.33.44","ttl":600},{"sub_domain":"","record_type":"MX","record_line":"默认","value":"mx.qq.com","ttl":600,"MX":10}]&domain_id=25556994,25556952'

返回参考：

    * JSON::

        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2015-09-08 17:38:09"
            },
            "job_id": "49590",
            "detail": [
                {
                    "id": 0,
                    "domain_id": "16",
                    "domain": "wuhongsheng.com",
                    "domain_grade": "DP_Free",
                    "err_msg": null,
                    "status": "waiting",
                    "operation": null,
                    "records": [
                        {
                            "id": 0,
                            "sub_domain": "www",
                            "record_type": "A",
                            "area": "默认",
                            "value": "11.22.33.44",
                            "mx": "5",
                            "ttl": "600",
                            "err_msg": null,
                            "status": "waiting",
                            "operation": "plus"
                        },
                        {
                            "id": 1,
                            "sub_domain": "wap",
                            "record_type": "A",
                            "area": "默认",
                            "value": "11.22.33.44",
                            "mx": "5",
                            "ttl": "600",
                            "err_msg": null,
                            "status": "waiting",
                            "operation": "plus"
                        },
                        {
                            "id": 2,
                            "sub_domain": "bbs",
                            "record_type": "A",
                            "area": "默认",
                            "value": "11.22.33.44",
                            "mx": "5",
                            "ttl": "600",
                            "err_msg": null,
                            "status": "waiting",
                            "operation": "plus"
                        },
                        {
                            "id": 3,
                            "sub_domain": "",
                            "record_type": "MX",
                            "area": "默认",
                            "value": "mx.qq.com",
                            "mx": "5",
                            "ttl": "600",
                            "err_msg": null,
                            "status": "waiting",
                            "operation": "plus"
                        }
                    ]
                },
                {
                    "id": 1,
                    "domain_id": "20",
                    "domain": "dnspod.com",
                    "domain_grade": "D_Ultra",
                    "err_msg": null,
                    "status": "waiting",
                    "operation": null,
                    "records": [
                        {
                            "id": 0,
                            "sub_domain": "www",
                            "record_type": "A",
                            "area": "默认",
                            "value": "11.22.33.44",
                            "mx": "5",
                            "ttl": "600",
                            "err_msg": null,
                            "status": "waiting",
                            "operation": "plus"
                        },
                        {
                            "id": 1,
                            "sub_domain": "wap",
                            "record_type": "A",
                            "area": "默认",
                            "value": "11.22.33.44",
                            "mx": "5",
                            "ttl": "600",
                            "err_msg": null,
                            "status": "waiting",
                            "operation": "plus"
                        },
                        {
                            "id": 2,
                            "sub_domain": "bbs",
                            "record_type": "A",
                            "area": "默认",
                            "value": "11.22.33.44",
                            "mx": "5",
                            "ttl": "600",
                            "err_msg": null,
                            "status": "waiting",
                            "operation": "plus"
                        },
                        {
                            "id": 3,
                            "sub_domain": "",
                            "record_type": "MX",
                            "area": "默认",
                            "value": "mx.qq.com",
                            "mx": "5",
                            "ttl": "600",
                            "err_msg": null,
                            "status": "waiting",
                            "operation": "plus"
                        }
                    ]
                }
            ]
        }

    * 字段说明:
        * job_id: 任务ID，批量操作为异步执行，拿到 job_id 说明批量操作已经成功加入到任务队列，可通过 Batch.Detail 接口异步去查询执行结果
        * detail: 任务详情，仅将用户的输入补全，并非实际执行结果，开发者可以忽略该字段


.. _Batch.Record.Modify:

批量修改记录
------------
接口地址：
    * https://dnsapi.cn/Batch.Record.Modify
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * record_id 记录的ID，多个 record_id 用英文的逗号分割
    * change 要修改的字段，可选值为 ["sub_domain"、"record_type"、"area"、"value"、"mx"、"ttl"、"status"] 中的某一个
    * change_to 修改为，具体依赖 change 字段，必填参数
    * value 要修改到的记录值，可选，仅当 change 字段为 "record_type" 时为必填参数
    * mx MX记录优先级，可选，仅当修改为 MX 记录时为必填参数
响应代码：
    * 共通返回【具体错误信息参见 message 字段】

示例:
    * 批量将多条记录修改为 CNAME 记录
        curl -X POST https://dnsapi.cn/Batch.Record.Modify -d 'login_token=LOGIN_TOKEN&format=json&record_id=36,90,91&change=record_type&change_to=CNAME&value=dnsapi1.cn.'
    * 批量将多条记录暂停
        curl -X POST https://dnsapi.cn/Batch.Record.Modify -d 'login_token=LOGIN_TOKEN&format=json&record_id=36,90,91&change=status&change_to=disable'

返回参考：

    * JSON::

        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2015-09-08 18:21:13"
            },
            "job_id": "409952",
            "detail": [
                {
                    "domain_id": 92,
                    "domain": "dnsapi1.cn",
                    "err_msg": null,
                    "status": "waiting",
                    "operation": null,
                    "records": [
                        {
                            "record_id": "36",
                            "sub_domain": "bbs",
                            "area": "默认",
                            "record_type": "CNAME",
                            "ttl": "600",
                            "value": "bbs.dnsapi2.com.",
                            "enabled": "1",
                            "status": "waiting",
                            "err_msg": null,
                            "id": 0,
                            "operation": "edit"
                        }
                    ],
                    "domain_grade": "D_Plus",
                    "id": 0
                },
                {
                    "domain_id": 171,
                    "domain": "dnsapi1.cn",
                    "err_msg": null,
                    "status": "waiting",
                    "operation": null,
                    "records": [
                        {
                            "record_id": "90",
                            "sub_domain": "www",
                            "area": "电信",
                            "record_type": "A",
                            "ttl": "3600",
                            "value": "11.11.22.22",
                            "enabled": "1",
                            "status": "waiting",
                            "err_msg": null,
                            "id": 0,
                            "operation": "edit"
                        },
                        {
                            "record_id": "91",
                            "sub_domain": "@",
                            "area": "电信",
                            "record_type": "MX",
                            "ttl": "3600",
                            "value": "61.144.40.92.",
                            "enabled": "1",
                            "status": "waiting",
                            "mx": "5",
                            "err_msg": null,
                            "id": 1,
                            "operation": "edit"
                        }
                    ],
                    "domain_grade": "D_Free",
                    "id": 1
                }
            ]
        }

    * 字段说明:
        * job_id: 任务ID，批量操作为异步执行，拿到 job_id 说明批量操作已经成功加入到任务队列，可通过 Batch.Detail 接口异步去查询执行结果
        * detail: 任务详情，仅将用户的输入补全，并非实际执行结果，开发者可以忽略该字段


.. _Batch.Detail:

获取任务详情
------------
接口地址：
    * https://dnsapi.cn/Batch.Detail
HTTP请求方式：
    * POST
请求参数：
    * 公共参数
    * job_id 任务ID
响应代码：
    * 共通返回
    * 8 数据为空，没有查到该任务

示例:: 

    curl -X POST https://dnsapi.cn/Batch.Detail -d 'login_token=LOGIN_TOKEN&format=json&job_id=409752'

返回参考：

    * JSON::
        
        {
            "status": {
                "code": "1",
                "message": "Action completed successful",
                "created_at": "2015-09-08 18:37:01"
            },
            "detail": [
                {
                    "id": 0,
                    "domain": "dnsapi1.cn",
                    "domain_grade": "DP_Free",
                    "err_msg": null,
                    "status": "ok",
                    "operation": "plus",
                    "records": [
                        {
                            "id": 0,
                            "sub_domain": "www",
                            "record_type": "A",
                            "area": "默认",
                            "value": "11.22.33.45",
                            "ttl": 600,
                            "err_msg": null,
                            "status": "ok",
                            "operation": "plus",
                            "record_id": 118332371,
                            "mx": null
                        },
                        {
                            "id": 1,
                            "sub_domain": "@",
                            "record_type": "A",
                            "area": "默认",
                            "value": "11.22.33.45",
                            "ttl": 600,
                            "err_msg": null,
                            "status": "ok",
                            "operation": "plus",
                            "record_id": 118332372,
                            "mx": null
                        }
                    ],
                    "domain_id": "25562928"
                },
                {
                    "id": 1,
                    "domain": "dnsapi2.cn",
                    "domain_grade": "DP_Free",
                    "err_msg": null,
                    "status": "ok",
                    "operation": "plus",
                    "records": [
                        {
                            "id": 0,
                            "sub_domain": "www",
                            "record_type": "A",
                            "area": "默认",
                            "value": "11.22.33.45",
                            "ttl": 600,
                            "err_msg": null,
                            "status": "ok",
                            "operation": "plus",
                            "record_id": 118332377,
                            "mx": null
                        },
                        {
                            "id": 1,
                            "sub_domain": "@",
                            "record_type": "A",
                            "area": "默认",
                            "value": "11.22.33.45",
                            "ttl": 600,
                            "err_msg": null,
                            "status": "ok",
                            "operation": "plus",
                            "record_id": 118332378,
                            "mx": null
                        }
                    ],
                    "domain_id": "25562929"
                }
            ],
            "total": 6,
            "success": 6,
            "fail": 0,
            "type": "domain",
            "created_at": "2015-09-08 18:36:43"
        }

    * 字段说明:
        * detail.status 和 detail.records.status 对应 ['error','ok','running','waiting'] 中的一种，分别代表出错、执行成功、正在执行中、等待执行，如果为 "error"，则在 err_msg 中会有错误信息
        * total: 执行的总条数，单个域名、单个记录均计数为1
        * success: 成功的条数
        * fail: 失败的条数
        * type: 对应 ['domain','record_add','record_modify'] 中的一种，分别代表批量添加域名、批量添加记录、批量修改记录
