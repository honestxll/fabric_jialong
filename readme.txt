这是工程的readme.txt文件.
主要过程记录
根据jl_alarm_log建立模型
CREATE TABLE `jl_alarm_log` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `site_id` int(11) DEFAULT NULL COMMENT '站房id',
  `notice_type` int(11) DEFAULT NULL COMMENT '通知类型',
  `alarm_detail` varchar(255) DEFAULT NULL COMMENT '报警详情',
  `alarm_level` int(11) DEFAULT NULL COMMENT '告警级别[1:一级告警  2:一级告警  3:一级告警  4:一级告警]',
  `site_type` int(11) DEFAULT NULL COMMENT '站房类型（1:配电室 2 开关站 3 环网站）',
  `trigger_value` varchar(255) DEFAULT NULL COMMENT '报警触发值',
  `alarm_device` int(11) DEFAULT NULL COMMENT '告警设备id',
  `alarm_param` int(11) DEFAULT NULL COMMENT '告警参数id',
  `alarm_time` datetime DEFAULT NULL COMMENT '报警时间',
  `auditor` int(11) DEFAULT NULL COMMENT '审批人 关联sys_user',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=13 DEFAULT CHARSET=utf8 COMMENT='报警记录表';

由于需要向分类账本中保存数据，所以必须设计相关的结构体用于声明要保存的数据结构，用于方便的在应用中处理数据。
Alarm结构体设计如下：
    ObjectType      string
    Id              string      记录id
    SiteId          string      站房id
    NoticeType      string      通知类型
    AlarmDetail     string      报警详情
    AlarmLevel      string      告警级别
    SiteType        string      站房类型
    TriggerValue    string      报警触发值
    AlarmParam      string      告警设备id
    AlarmTime       string      报警时间
    Auditor         string      审批人
    Historys        []HistoryItem

为了能够从当前的分类状态中查询出详细的历史操作记录，我们在 Alarm 中设计了一个类型为HistoryItem 数组的 Historys 成员，表示当前状态的历史记录集。

HistoryItem 结构体设计如下表所示：
TxId    string      交易编号
Alarm   Alarm       本次历史记录的详细信息


创建 chaincode/alarmStruct.go 定义结构体
创建 chaincode/alarmCC 定义PutAlarm addAlarm
创建 main.go 重写invoke Init方法 定义启动链码的方法
创建 config.yaml作为全局配置文件
创建 sdkInit/main.go 定义SetupSDK CreateChannel方法
创建 main.go 测试文件 测试启动网络启动SDK 创建通道是否正常
创建Makefile
提交代码
清空网络
切换到目标目录 git clone 代码 重命名
启动网络

2.12
目标是在education同级别目录创建一个alarm目录 提供报警添加接口 实现对报警数据的保存.
fixtures还是用以前的代码 考虑到这是秘钥相关的文件 暂时复用
创建Alarm结构体 对结构体的操作只保留添加逻辑 addAlarm 调用 PutAlarm
start.go里面包含启动sdk 创建通道
main.go测试链码文件的启动sdk 创建通道逻辑
代码提交到git地址为https://github.com/1250446609/fabric_jialong.git
因为现在的目录变成了 /home/ubuntu/go/src/github.com/kongyixueyuan.com/alarm
所以在import相关的地方 makefile config.yaml文件里面目录中的education都变成了alarm
把项目克隆到education同级目录 make清空网络
执行docker-compose up -d启动网络正常
go build报错

ubuntu@VM-16-13-ubuntu:~/go/src/github.com/kongyixueyuan.com/alarm$ go build
can't load package: package github.com/kongyixueyuan.com/alarm: no Go files in /home/ubuntu/go/src/github.com/kongyixueyuan.com/alarm
ubuntu@VM-16-13-ubuntu:~/go/src/github.com/kongyixueyuan.com/alarm$ go build
can't load package: package github.com/kongyixueyuan.com/alarm: no Go files in /home/ubuntu/go/src/github.com/kongyixueyuan.com/alarm
ubuntu@VM-16-13-ubuntu:~/go/src/github.com/kongyixueyuan.com/alarm$ go mod init alarm
go: creating new go.mod: module alarm
ubuntu@VM-16-13-ubuntu:~/go/src/github.com/kongyixueyuan.com/alarm$ go build
can't load package: package .: no Go files in /home/ubuntu/go/src/github.com/kongyixueyuan.com/alarm
ubuntu@VM-16-13-ubuntu:~/go/src/github.com/kongyixueyuan.com/alarm$

这个貌似不是go mod里面确实replace的原因






