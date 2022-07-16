# xueliuhongzhong
1. 功能设计
发牌
```
client -------------------->  server  ---------------------------> redis
1. client提供clientId给server
2. server通过clientId匹配是否有已经开房，如果开房则返回connectionId,跳过下面所有步骤
3. server将clientId放入redis的list中，等待匹配，如果满4个,则组局，返回对应的connectionId，跳过下面所有步骤
4. 不满足4个，返回对应的状态给到client,用于下次轮训
5. client获取结果后，判断是否重复第1步
```

```
redis数据结构如下
clientIds : [c1,c2,c3,c4,c5.....]
connectionId_${id} : [1,a,b,A,.....] // 存牌局
clientId_${id}: {connectionId_${id}} // 反向查询是否有组局邀请
```
