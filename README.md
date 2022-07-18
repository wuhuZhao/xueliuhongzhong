# xueliuhongzhong
1. 功能设计
开局
```
client -------------------->  server  ---------------------------> redis
1. client提供clientId给server
2. server通过clientId匹配是否有已经开房，如果开房则返回connectionId和牌局的所有信息（牌的顺序，每个client自己的牌，牌河的牌，哪个client拿牌）,跳过下面所有步骤
3. server将clientId放入redis的list中，等待匹配，如果满4个,则组局，返回对应的connectionId，跳过下面所有步骤
4. 不满足4个，返回对应的状态给到client,用于下次轮训
5. client获取结果后，判断是否重复第1步
```

```
redis数据结构如下
clientIds : [c1,c2,c3,c4,c5.....]
connectionId_${id} : {total:[1,a,b,A,.....],clientId1: [1,a,b], client2:[] client3:[],paihe: [], cur: int} // 存牌局
clientId_${id}: {connectionId_${id}} // 反向查询是否有组局邀请
```
交换3张牌



发牌
client -------------------->  server  ---------------------------> redis
for true:
    1. client提供connectionId和clientId调用发牌接口
    2. server收到发牌接口后，查询本地缓存是否为当前client拿牌，返回对应的clientId，然后异步同步当前牌序信息，如果clientId对应上，client自己获取本地接口返回的牌，添加到自己的牌中，如果clientId对应不上，则不摸牌


