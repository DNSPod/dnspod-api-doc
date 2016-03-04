DNSPod用户API文档
=================

.. centered:: DNSPod Documentation |version| released |today|


最近更新
---------
2016-03-04
    * 增加 Domain.Acquiresend.New 接口，直接将域名取回的验证链接发送至 WHOIS 邮箱，用户点击链接即可完成取回
    * 将示例中的 login_email、login_password 参数更换为 login_token，推荐开发者们使用 API Token 进行鉴权
2015-11-24
    * 增加解析记录权重
2015-09-09
    * 增加批量操作的相关文档
2015-05-15
    * 增加 Token 的验证机制说明
    * Record.Create 和 Record.Modify 增加 status 参数，允许传入记录的初始状态
2014-09-02
    * Domain.List 和 Record.List 增加 keyword 参数，实现搜索功能
2014-08-29
    * 紧急更新，旧QQ群 176455159 已经废弃，新开发者交流群为：150138660，欢迎加入
2014-07-22
    * 增加了 Record.Modify 和 Record.Ddns 的关于滥用的说明
2014-06-02
    * 新增了帐号安全、记录安全、域名过户相关的错误码
    * 修正部分错误码说明文字
2013-05-14
    * Record.Ddns 增加可选的value参数，允许用户提交自己的IP地址
    * 增加新套餐等级相关的说明
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
    batch
    d-monitor
    custom-monitoring
