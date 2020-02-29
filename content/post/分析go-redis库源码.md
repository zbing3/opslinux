---
title: 分析go-redis库源码
date: 2016-10-30 08:43:42
categories:
tags:
---

项目地址：https://github.com/go-redis/redis

# 寻找入口

寻找入库，一般从 Quickstart 会给我们很多线索


```
func ExampleNewClient() {
    client := redis.NewClient(&redis.Options{
        Addr:     "localhost:6379",
        Password: "", // no password set
        DB:       0,  // use default DB
    })

    pong, err := client.Ping().Result()
    fmt.Println(pong, err)
    // Output: PONG <nil>
}

```
我们看到 创建client的时候调用 redis.NewClient 方法，我们不妨寻找一下这个函数

```
$ git clone https://github.com/go-redis/redis.git

$ cd redis

$ grep -r -n "NewClient" *.go                                                                                                                                     [10:17:37]
bench_test.go:12:	client := redis.NewClient(&redis.Options{
cluster.go:239:		Client: NewClient(opt),
cluster_test.go:64:		client := redis.NewClient(&redis.Options{
command_test.go:14:		client = redis.NewClient(redisOptions())
commands_test.go:19:		client = redis.NewClient(redisOptions())
example_test.go:15:	client = redis.NewClient(&redis.Options{
example_test.go:26:func ExampleNewClient() {
example_test.go:27:	client := redis.NewClient(&redis.Options{
iterator_test.go:47:		client = redis.NewClient(redisOptions())
main_test.go:204:	client := redis.NewClient(&redis.Options{
pipeline_test.go:17:		client = redis.NewClient(redisOptions())
pool_test.go:17:		client = redis.NewClient(redisOptions())
pubsub_test.go:19:		client = redis.NewClient(redisOptions())
race_test.go:23:		client = redis.NewClient(redisOptions())
race_test.go:114:		client = redis.NewClient(redisOptions())
race_test.go:175:			client := redis.NewClient(opt)
race_test.go:198:			client := redis.NewClient(opt)
redis.go:167:// NewClient returns a client to the Redis Server specified by Options.
redis.go:168:func NewClient(opt *Options) *Client {
redis_test.go:17:		client = redis.NewClient(redisOptions())
redis_test.go:40:		custom := redis.NewClient(&redis.Options{
redis_test.go:109:		db2 := redis.NewClient(&redis.Options{
redis_test.go:141:		client = redis.NewClient(&redis.Options{
redis_test.go:195:		client = redis.NewClient(redisOptions())
ring.go:157:		ring.addClient(name, NewClient(clopt))
tx_test.go:17:		client = redis.NewClient(redisOptions())


```

我们可以看到 redis.go 的168行有 NewClient 函数的定义

```
redis.go:168:func NewClient(opt *Options) *Client {
```

这样我们找到了一个入口


# 绘制项目地图

我们找查看一个项目，绘制这个项目的地图很重要

通过 redis.go 168行我们可以看到，输入的参数 Options 和 返回值 Client
![](media/14777950888230.jpg)


我们看下 Optins 的定义：

```
type Options struct {
	Network string
	Addr string
	Dialer func() (net.Conn, error)
	Password string
	DB int
	MaxRetries int
	DialTimeout time.Duration
	ReadTimeout time.Duration
	WriteTimeout time.Duration
	PoolSize int
	PoolTimeout time.Duration
	IdleTimeout time.Duration
	IdleCheckFrequency time.Duration
	ReadOnly bool
}
```
都是一些连接是用的参数，往下看有初始化函数

```
func (opt *Options) init() {
	if opt.Network == "" {
		opt.Network = "tcp"
	}
	if opt.Dialer == nil {
		opt.Dialer = func() (net.Conn, error) {
			return net.DialTimeout(opt.Network, opt.Addr, opt.DialTimeout)
		}
	}
	if opt.PoolSize == 0 {
		opt.PoolSize = 10
	}
	if opt.DialTimeout == 0 {
		opt.DialTimeout = 5 * time.Second
	}
	if opt.ReadTimeout == 0 {
		opt.ReadTimeout = 3 * time.Second
	}
	if opt.WriteTimeout == 0 {
		opt.WriteTimeout = opt.ReadTimeout
	}
	if opt.PoolTimeout == 0 {
		opt.PoolTimeout = opt.ReadTimeout + time.Second
	}
	if opt.IdleTimeout == 0 {
		opt.IdleTimeout = 5 * time.Minute
	}
	if opt.IdleCheckFrequency == 0 {
		opt.IdleCheckFrequency = time.Minute
	}
}
```
对于参数提供了默认值

然后我们看一下 Clinet 的定义， redis.go 151行 ：

```
type Client struct {
	baseClient
	cmdable
}

var _ Cmdable = (*Client)(nil)
```


![](media/14778013264082.jpg)

Client 是由 baseClient 和 cmdable 组合而成。 从命名上来看 baseClient是保存一些基础不变的东西，cmdable可能是负责命令。


```
var _ Cmdable = (*Client)(nil)
```
这行的意思 Client 要实现 Cmdable 这样的接口，要不然编译器会报错。 从 Client的结构上看只有 cmdble才能实现这样的一个接口

我们看下 Cmdable 的源码：


```
type Cmdable interface {
	Pipeline() *Pipeline
	Pipelined(fn func(*Pipeline) error) ([]Cmder, error)

	Echo(message interface{}) *StringCmd
	Ping() *StatusCmd
	Quit() *StatusCmd
	Del(keys ...string) *IntCmd
	Dump(key string) *StringCmd
	Exists(key string) *BoolCmd
	Expire(key string, expiration time.Duration) *BoolCmd
	ExpireAt(key string, tm time.Time) *BoolCmd
	Keys(pattern string) *StringSliceCmd
	Migrate(host, port, key string, db int64, timeout time.Duration) *StatusCmd
	Move(key string, db int64) *BoolCmd
	ObjectRefCount(keys ...string) *IntCmd
	ObjectEncoding(keys ...string) *StringCmd
	ObjectIdleTime(keys ...string) *DurationCmd
	Persist(key string) *BoolCmd
	PExpire(key string, expiration time.Duration) *BoolCmd
	PExpireAt(key string, tm time.Time) *BoolCmd
	PTTL(key string) *DurationCmd
	RandomKey() *StringCmd
	Rename(key, newkey string) *StatusCmd
	RenameNX(key, newkey string) *BoolCmd
	Restore(key string, ttl time.Duration, value string) *StatusCmd
	RestoreReplace(key string, ttl time.Duration, value string) *StatusCmd
	Sort(key string, sort Sort) *StringSliceCmd
	SortInterfaces(key string, sort Sort) *SliceCmd
	TTL(key string) *DurationCmd
	Type(key string) *StatusCmd
	Scan(cursor uint64, match string, count int64) Scanner
	SScan(key string, cursor uint64, match string, count int64) Scanner
	HScan(key string, cursor uint64, match string, count int64) Scanner
	ZScan(key string, cursor uint64, match string, count int64) Scanner
	Append(key, value string) *IntCmd
	BitCount(key string, bitCount *BitCount) *IntCmd
	BitOpAnd(destKey string, keys ...string) *IntCmd
	BitOpOr(destKey string, keys ...string) *IntCmd
	BitOpXor(destKey string, keys ...string) *IntCmd
	BitOpNot(destKey string, key string) *IntCmd
	BitPos(key string, bit int64, pos ...int64) *IntCmd
	Decr(key string) *IntCmd
	DecrBy(key string, decrement int64) *IntCmd
	Get(key string) *StringCmd
	GetBit(key string, offset int64) *IntCmd
	GetRange(key string, start, end int64) *StringCmd
	GetSet(key string, value interface{}) *StringCmd
	Incr(key string) *IntCmd
	IncrBy(key string, value int64) *IntCmd
	IncrByFloat(key string, value float64) *FloatCmd
	MGet(keys ...string) *SliceCmd
	MSet(pairs ...interface{}) *StatusCmd
	MSetNX(pairs ...interface{}) *BoolCmd
	Set(key string, value interface{}, expiration time.Duration) *StatusCmd
	SetBit(key string, offset int64, value int) *IntCmd
	SetNX(key string, value interface{}, expiration time.Duration) *BoolCmd
	SetXX(key string, value interface{}, expiration time.Duration) *BoolCmd
	SetRange(key string, offset int64, value string) *IntCmd
	StrLen(key string) *IntCmd
	HDel(key string, fields ...string) *IntCmd
	HExists(key, field string) *BoolCmd
	HGet(key, field string) *StringCmd
	HGetAll(key string) *StringStringMapCmd
	HIncrBy(key, field string, incr int64) *IntCmd
	HIncrByFloat(key, field string, incr float64) *FloatCmd
	HKeys(key string) *StringSliceCmd
	HLen(key string) *IntCmd
	HMGet(key string, fields ...string) *SliceCmd
	HMSet(key string, fields map[string]string) *StatusCmd
	HSet(key, field, value string) *BoolCmd
	HSetNX(key, field, value string) *BoolCmd
	HVals(key string) *StringSliceCmd
	BLPop(timeout time.Duration, keys ...string) *StringSliceCmd
	BRPop(timeout time.Duration, keys ...string) *StringSliceCmd
	BRPopLPush(source, destination string, timeout time.Duration) *StringCmd
	LIndex(key string, index int64) *StringCmd
	LInsert(key, op string, pivot, value interface{}) *IntCmd
	LInsertBefore(key string, pivot, value interface{}) *IntCmd
	LInsertAfter(key string, pivot, value interface{}) *IntCmd
	LLen(key string) *IntCmd
	LPop(key string) *StringCmd
	LPush(key string, values ...interface{}) *IntCmd
	LPushX(key string, value interface{}) *IntCmd
	LRange(key string, start, stop int64) *StringSliceCmd
	LRem(key string, count int64, value interface{}) *IntCmd
	LSet(key string, index int64, value interface{}) *StatusCmd
	LTrim(key string, start, stop int64) *StatusCmd
	RPop(key string) *StringCmd
	RPopLPush(source, destination string) *StringCmd
	RPush(key string, values ...interface{}) *IntCmd
	RPushX(key string, value interface{}) *IntCmd
	SAdd(key string, members ...interface{}) *IntCmd
	SCard(key string) *IntCmd
	SDiff(keys ...string) *StringSliceCmd
	SDiffStore(destination string, keys ...string) *IntCmd
	SInter(keys ...string) *StringSliceCmd
	SInterStore(destination string, keys ...string) *IntCmd
	SIsMember(key string, member interface{}) *BoolCmd
	SMembers(key string) *StringSliceCmd
	SMove(source, destination string, member interface{}) *BoolCmd
	SPop(key string) *StringCmd
	SPopN(key string, count int64) *StringSliceCmd
	SRandMember(key string) *StringCmd
	SRandMemberN(key string, count int64) *StringSliceCmd
	SRem(key string, members ...interface{}) *IntCmd
	SUnion(keys ...string) *StringSliceCmd
	SUnionStore(destination string, keys ...string) *IntCmd
	ZAdd(key string, members ...Z) *IntCmd
	ZAddNX(key string, members ...Z) *IntCmd
	ZAddXX(key string, members ...Z) *IntCmd
	ZAddCh(key string, members ...Z) *IntCmd
	ZAddNXCh(key string, members ...Z) *IntCmd
	ZAddXXCh(key string, members ...Z) *IntCmd
	ZIncr(key string, member Z) *FloatCmd
	ZIncrNX(key string, member Z) *FloatCmd
	ZIncrXX(key string, member Z) *FloatCmd
	ZCard(key string) *IntCmd
	ZCount(key, min, max string) *IntCmd
	ZIncrBy(key string, increment float64, member string) *FloatCmd
	ZInterStore(destination string, store ZStore, keys ...string) *IntCmd
	ZRange(key string, start, stop int64) *StringSliceCmd
	ZRangeWithScores(key string, start, stop int64) *ZSliceCmd
	ZRangeByScore(key string, opt ZRangeBy) *StringSliceCmd
	ZRangeByLex(key string, opt ZRangeBy) *StringSliceCmd
	ZRangeByScoreWithScores(key string, opt ZRangeBy) *ZSliceCmd
	ZRank(key, member string) *IntCmd
	ZRem(key string, members ...interface{}) *IntCmd
	ZRemRangeByRank(key string, start, stop int64) *IntCmd
	ZRemRangeByScore(key, min, max string) *IntCmd
	ZRevRange(key string, start, stop int64) *StringSliceCmd
	ZRevRangeWithScores(key string, start, stop int64) *ZSliceCmd
	ZRevRangeByScore(key string, opt ZRangeBy) *StringSliceCmd
	ZRevRangeByLex(key string, opt ZRangeBy) *StringSliceCmd
	ZRevRangeByScoreWithScores(key string, opt ZRangeBy) *ZSliceCmd
	ZRevRank(key, member string) *IntCmd
	ZScore(key, member string) *FloatCmd
	ZUnionStore(dest string, store ZStore, keys ...string) *IntCmd
	PFAdd(key string, els ...interface{}) *IntCmd
	PFCount(keys ...string) *IntCmd
	PFMerge(dest string, keys ...string) *StatusCmd
	BgRewriteAOF() *StatusCmd
	BgSave() *StatusCmd
	ClientKill(ipPort string) *StatusCmd
	ClientList() *StringCmd
	ClientPause(dur time.Duration) *BoolCmd
	ClientSetName(name string) *BoolCmd
	ConfigGet(parameter string) *SliceCmd
	ConfigResetStat() *StatusCmd
	ConfigSet(parameter, value string) *StatusCmd
	DbSize() *IntCmd
	FlushAll() *StatusCmd
	FlushDb() *StatusCmd
	Info(section ...string) *StringCmd
	LastSave() *IntCmd
	Save() *StatusCmd
	Shutdown() *StatusCmd
	ShutdownSave() *StatusCmd
	ShutdownNoSave() *StatusCmd
	SlaveOf(host, port string) *StatusCmd
	Time() *TimeCmd
	Eval(script string, keys []string, args ...interface{}) *Cmd
	EvalSha(sha1 string, keys []string, args ...interface{}) *Cmd
	ScriptExists(scripts ...string) *BoolSliceCmd
	ScriptFlush() *StatusCmd
	ScriptKill() *StatusCmd
	ScriptLoad(script string) *StringCmd
	DebugObject(key string) *StringCmd
	PubSubChannels(pattern string) *StringSliceCmd
	PubSubNumSub(channels ...string) *StringIntMapCmd
	PubSubNumPat() *IntCmd
	ClusterSlots() *ClusterSlotsCmd
	ClusterNodes() *StringCmd
	ClusterMeet(host, port string) *StatusCmd
	ClusterForget(nodeID string) *StatusCmd
	ClusterReplicate(nodeID string) *StatusCmd
	ClusterResetSoft() *StatusCmd
	ClusterResetHard() *StatusCmd
	ClusterInfo() *StringCmd
	ClusterKeySlot(key string) *IntCmd
	ClusterCountFailureReports(nodeID string) *IntCmd
	ClusterCountKeysInSlot(slot int) *IntCmd
	ClusterDelSlots(slots ...int) *StatusCmd
	ClusterDelSlotsRange(min, max int) *StatusCmd
	ClusterSaveConfig() *StatusCmd
	ClusterSlaves(nodeID string) *StringSliceCmd
	ClusterFailover() *StatusCmd
	ClusterAddSlots(slots ...int) *StatusCmd
	ClusterAddSlotsRange(min, max int) *StatusCmd
	GeoAdd(key string, geoLocation ...*GeoLocation) *IntCmd
	GeoPos(key string, members ...string) *GeoPosCmd
	GeoRadius(key string, longitude, latitude float64, query *GeoRadiusQuery) *GeoLocationCmd
	GeoRadiusByMember(key, member string, query *GeoRadiusQuery) *GeoLocationCmd
	GeoDist(key string, member1, member2, unit string) *FloatCmd
	GeoHash(key string, members ...string) *StringSliceCmd
	Command() *CommandsInfoCmd
}
```

我们看到了 Cmdable 这个接口其实就是实现了 redis 命令的封装。

我们接着看 baseClient

```
type baseClient struct {
	connPool pool.Pooler
	opt      *Options

	onClose func() error // hook called when client is closed
}
```

baseClient 引用了连接池，


![](media/14778030546128.jpg)


我们接着往下看 redis.go 这个文件

```
func (c *baseClient) Process(cmd Cmder) error {
	for i := 0; i <= c.opt.MaxRetries; i++ {
		if i > 0 {
			cmd.reset()
		}

		cn, _, err := c.conn()
		if err != nil {
			cmd.setErr(err)
			return err
		}

		readTimeout := cmd.readTimeout()
		if readTimeout != nil {
			cn.ReadTimeout = *readTimeout
		} else {
			cn.ReadTimeout = c.opt.ReadTimeout
		}
		cn.WriteTimeout = c.opt.WriteTimeout

		if err := writeCmd(cn, cmd); err != nil {
			c.putConn(cn, err, false)
			cmd.setErr(err)
			if err != nil && internal.IsRetryableError(err) {
				continue
			}
			return err
		}

		err = cmd.readReply(cn)
		c.putConn(cn, err, readTimeout != nil)
		if err != nil && internal.IsRetryableError(err) {
			continue
		}

		return err
	}

	return cmd.Err()
}
```
发现 baseClient 有个 Process 方法 这个应该是处理执行的过程 需要我们注意。

![](media/14778049112136.jpg)


根据广度优先原则，我们再看 Client 上还有什么关联的内容

我们可以看到 redis.go 这个文件里面 还包含了 Pipeline 和 pubSub 两个函数


```
func (c *Client) Pipeline() *Pipeline {
}

func (c *Client) pubSub() *PubSub {
}
```

![](media/14778093207234.jpg)

然后我们再看下，项目文件，还有什么我们并没有涉及到的


```
 $ ls                                                                                                                     
CHANGELOG.md   cluster_client_test.go  doc.go            main_test.go      pubsub.go       ring.go           tx.go
LICENSE        cluster_test.go         example_test.go   options.go        pubsub_test.go  ring_test.go      tx_test.go
Makefile       command.go              export_test.go    parser.go         race_test.go    script.go
README.md      command_test.go         internal/         pipeline.go       redis.go        sentinel.go
bench_test.go  commands.go             iterator.go       pipeline_test.go  redis_test.go   sentinel_test.go
cluster.go     commands_test.go        iterator_test.go  pool_test.go      result.go       testdata/
```

答案就是 cluster，cluster 是连接 redis 集群的方式，会提供给用户使用

![](/media/14779019374152.jpg)


我们详细看一下 cluster 源码，cluster 里面有自己的 Options，值得注意的是，有 Node 逻辑


```
type clusterNode struct {
	Client  *Client
	Latency time.Duration
	loading time.Time
}

```
使用组合的方式把 Client 包装进来。


```

```

![](/media/14779085735522.jpg)



