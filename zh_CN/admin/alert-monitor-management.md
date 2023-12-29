# 监控告警管理

NeuronEX 提供监控告警功能，可帮助用户实时了解 NeuronEX 的状态，并及时发现异常情况。

## 监控

NeuronEX 提供多种指标数据来方便您监控，包括:

- 数据采集引擎指标，如南北向驱动数量，驱动连接状态，异常驱动等。
- 数据处理引擎指标，如规则记录输入,输出数量，停止规则数量等。
- 系统资源指标，如CPU,内存等。

用户可在下发监控配置时，可以指定需要哪些指标数据。目前提供两种方式获取到这些指标数据：

1. 配置[pushgatway](https://github.com/prometheus/pushgateway) 地址(Pushgateway 是一个 Prometheus 的组件，开源的系统监控、报警、时间序列数据库的组合)，NeuronEX将自动推送指标数据到指定的pushgatway中
2. 通过API 查询指标数据

NeuronEX默认关闭监控功能，用户可通过调用API来下发监控配置及查询相关内容，详情可查看[链接](https://docs.emqx.com/zh/neuronex/latest/api/api-docs.html#tag/monitor/operation/MetricConfig)。

## 告警

NeuronEX 通过轮询的方式来判断告警事件的发生，目前提供3种告警类型，用户下发告警相关配置后，可以指定需要哪些规则，以及每种告警事件触发或者恢复时连续监控的次数（即N值，P值），NeuronEX会根据配置生成告警触发或告警恢复事件。

| 告警类型                                 | 告警对象         | 告警触发条件                          | 告警触发事件生成条件 | 告警恢复事件生成条件  |
| ---------------------------------------- | ---------------- | ------------------------------------- | -------------------- | --------------------- |
| 数采驱动节点异常告警  （包括南向、北向） | 单个驱动         | 驱动处于运行中但未连接状态            | 连续监控N次          | 连续监控非异常状态P次 |
| 流处理引擎规则异常告警                   | 单个规则         | 一个规则的任意source, op,sink异常增加 | 连续监控N次          | 连续监控非异常状态P次 |
| NeuronEX重启告警                         | 当前NeuronEX实例 | NeuronEX重启                          | 监控到1次            | 无                    |

目前提供两种方式获取到这些告警事件：

1. 配置webhook地址，NeuronEX将自动推送告警事件到指定webhook中。
2. 通过API查询最近产生的告警事件。

NeuronEX默认关闭告警功能，用户可通过调用API来下发告警配置及查询相关内容，详情可查看[链接](https://docs.emqx.com/zh/neuronex/latest/api/api-docs.html#tag/monitor/operation/AlertRuleConfig)。

## 监控告警状态

用户可在管理下的系统信息页面查看到当前“日志推送状态“、”监控推送状态“、”告警推送状态“。