## 消息队列

1.  消息队列的优缺点
    1.  优点
        1.  解耦
        2.  异步
        3.  削峰
    2.  缺点
        1.  系统可用性降低
            1.  系统引入的外部依赖越多,越容易挂掉,一旦MQ故障,整个系统将会无法运转
        2.  系统复杂性增加
            1.  需要考虑重复消费/消息积压/消息丢失等问题
        3.  一致性问题
            1.  多个系统中有些成功有些失败,但请求可能已经返回成功了