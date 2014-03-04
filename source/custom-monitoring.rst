自定义监控相关
==============

使用开源软件 collectd 上报系统指标
----------------------------------
collectd 下载页： http://collectd.org/download.shtml     

以源码安装 collectd 为例::

    wget http://collectd.org/files/collectd-5.4.1.tar.gz
    tar -xvzf collectd-5.4.1.tar.gz
    cd collectd-5.4.1
    ./configure && make && sudo make install

修改配置文件，指定上报服务器::

    sudo vim /opt/collectd/etc/collectd.conf

替换::

    #LoadPlugin write_graphite

为::

    LoadPlugin write_graphite

搜索 Plugin write_graphite 对应的配置块, 取消注释，并做相应的修改::

    <Plugin write_graphite>
      <Node "example">
        Host "collector.monitor.dnspod.cn"
        Port "2003"
        Protocol "tcp"
        LogSendErrors true
        Prefix "授权码/域名/IP/"   # 例如："xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/test.com/192.168.1.111
        Postfix ""
        StoreRates true
        AlwaysAppendDS false
        EscapeCharacter "_"
      </Node>
    </Plugin>  

启动 collectd::

    sudo /opt/collectd/sbin/collectd


如何使用 collectd 收集更多的服务器指标
--------------------------------------
例如，我想监控服务器的磁盘空间使用情况::

    sudo vim /opt/collectd/etc/collectd.conf

替换::

    #LoadPlugin df

为::

    LoadPlugin df 

搜索 LoadPlugin df 对应的配置块，取消注释，并做相应的修改::

    <Plugin df>
            Device "/dev/xvda1"    # 需根据系统配置做相应修改
            MountPoint "/"         # 需根据系统配置做相应修改
            FSType "ext4"          # 需根据系统配置做相应修改
            IgnoreSelected false
            ReportByDevice false
            ReportReserved false
            ReportInodes false
            ValuesAbsolute false
            ValuesPercentage true
    </Plugin>
 
了解更多指标收集插件，请参考 https://collectd.org/wiki/index.php/Table_of_Plugins 


使用 Shell 上报数据
-------------------
TCP 方式::

   echo "YOUR_API_KEY/test.com/192.168.1.111/active-users 200 $(date +%s)" | nc collector.monitor.dnspod.cn 2003 -q 1; echo


一些有爱的上报脚本
------------------
* https://github.com/onlytiancai/codesnip/blob/master/shell/collect.sh

