DNSPod用户API文档
=================

.. centered:: DNSPod Documentation |version| released |today|


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

目录
-----
.. toctree::
    :maxdepth: 2

    info
    accounts
    domains
    records
    d-monitor
