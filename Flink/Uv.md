# UV类统计

- DAU: Daily Active User 日活跃用户

- UV: Unique Visitor 独立游客

- 在某些场景两者是一样的，也有可能不同，DAU统计用户id，Uv统计设备Id或其他id


### 工具

- bloomfilter：将key哈希到bit-map上，如果这个bit位不存在，则update该位置的bit，标明已经来过了
    由于哈希冲突的存在，不同的key可能哈希到相同的位置，因此bloomfilter只能肯定这个key不存在，却
    不能保证key一定存在（例如有一个key哈希到的位置已经存在了，由于哈希冲突的存在，不确定这个key是
    唯一来过这里的，bloomfilter只能报这个key存在了，其实这个key可能不存在，因为hash冲突，他没有位置
    进行判断了）。

- 哈希碰撞是指，两个不同的输入得到了相同的输出：

### 根据nginx_time进行天、时等时间窗口的统计

- 设计好去重的key(时间统计粒度前缀 + 业务主键)：yyyyMMddHH + uid
- redis bloonfilter全局去重，利用bloomfilter判断这个key是否已经存在了，不存在则Uv+1
- 往往在全局去重前会在本地去一次重，减轻redis压力。