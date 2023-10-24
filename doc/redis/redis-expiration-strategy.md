## redis的过期数据删除策略

1. 定期删除
   1. redis默认各隔100ms就随机抽取设置了过期时间的key,检查是否过期,如果过期就删除
2. 惰性删除
   1. 在获取key时,先检查key是否过期,过期就删除
3. 内存淘汰机制
   1. no-enviction：新写入操作报错
   2. **allkeys-lru： 在key空间中，移除最近最少使用的**
   3. keyallkeys-random: 在key空间中，随机移除某个
   4. keyvolatile-lru： 在设置了过期时间的key空间中，移除最近最少使用的
   5. keyvolatile-random： 在设置了过期时间的key空间中，随机移除某个
   6. keyvolatile-ttl： 在设置了过期时间的key空间中，有更早过期时间的key优先移除