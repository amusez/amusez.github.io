## redis数据丢失(异步复制/集群脑裂)

1.  数据丢失原因
    1.  因为主从复制的异步的,所以可能数据还没复制到slave,但是master已经宕机
    2.  集群脑裂: 某个master所在机器脱离了正常网络,跟其他slave不能连接,但实际上master还在运行,此时哨兵认为master宕机,开启选举,将其他slave切换成了master,此时集群中就有两个master,也就是脑裂,但是client可能还在连接原来的master,当原来的master网络恢复时,会被作为一个slave挂到新的master上去,自己的数据会清空,重新从新的master复制数据
    3.  解决方案
        1.  min-slaves-to-write 1
        2.  min-slaves-max-lag 10
            1.  要求至少有一个slave,数据复制和同步的延迟不能超过10s,如果说所有的slave都超过了10s,此时master不再接收新的请求
            2.  减少异步复制的数据丢失
                1.  有了min-slaves-max-lag这个配置,就可以确保一旦slave复制数据和ack延时太长,就认为master宕机后损失的数据太多了,就拒绝写请求,这样就可以把master宕机时由于部分数据未同步到slave的数据丢失降低到可控范围内
            3.  减少脑裂的数据丢失
                1.  通过上面两个配置来确保如果不能继续给指定数量的slave发送数据,而且slave超过10s没有给自己ack,那么直接拒绝客户端的写请求,也就避免了数据丢失在
        3.  redis不接收写请求时,可在客户端将数据暂时写入消息队列,每过一段时间,读取队列重新存入redis