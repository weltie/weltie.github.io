# ClickHouse-异常记录



Ephemeral nodes in ZooKeeper are special nodes that exist as long as the session that created the node is active. If the session ends, the ephemeral nodes belonging to that session are removed. It seems like the session that created the node in question has ended, hence the Session expired error.
You might want to check the zookeeper_session_expiration_check_period setting in your ClickHouse configuration. This setting controls the ZooKeeper session expiration check period, in seconds. The default value is 60 seconds. If the value is too high, it might lead to sessions expiring before operations are complete.
```
{} <Error> zkutil::EphemeralNodeHolder::~EphemeralNodeHolder(): Cannot remove /clickhouse/tables/2938d44b-df6b-4fc0-8456-419b78a20b7d/5/replicas/9.0.16.10/is_active: : Code: 999. Coordination::Exception: Session expired (Session expired). (KEEPER_EXCEPTION), Stack trace (when copying this message, always include the lines below):

0. Poco::Exception::Exception(std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> > const&, int) @ 0x16f20933 in /usr/bin/clickhouse
1. DB::Exception::Exception(std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> > const&, int, bool) @ 0xf3b8d9a in /usr/bin/clickhouse
2. Coordination::Exception::Exception(std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> > const&, Coordination::Error, int) @ 0x1516d3d9 in /usr/bin/clickhouse
3. Coordination::Exception::Exception(std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> > const&, Coordination::Error) @ 0x1516d5ec in /usr/bin/clickhouse
4. Coordination::ZooKeeper::pushRequest(Coordination::ZooKeeper::RequestInfo&&) @ 0x151ad415 in /usr/bin/clickhouse
5. Coordination::ZooKeeper::remove(std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> > const&, int, std::__1::function<void (Coordination::RemoveResponse const&)>) @ 0x151ae144 in /usr/bin/clickhouse
6. zkutil::ZooKeeper::asyncTryRemoveNoThrow(std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> > const&, int) @ 0x15174052 in /usr/bin/clickhouse
7. zkutil::ZooKeeper::removeImpl(std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> > const&, int) @ 0x15173ce5 in /usr/bin/clickhouse
8. zkutil::ZooKeeper::tryRemove(std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> > const&, int) @ 0x1517422d in /usr/bin/clickhouse
9. zkutil::EphemeralNodeHolder::~EphemeralNodeHolder() @ 0xf4cffff in /usr/bin/clickhouse
10. DB::ReplicatedMergeTreeRestartingThread::partialShutdown(bool) @ 0x14be3ef4 in /usr/bin/clickhouse
11. DB::ReplicatedMergeTreeRestartingThread::runImpl() @ 0x14be3c1d in /usr/bin/clickhouse
12. DB::ReplicatedMergeTreeRestartingThread::run() @ 0x14be343f in /usr/bin/clickhouse
13. DB::BackgroundSchedulePoolTaskInfo::execute() @ 0x134cfe2e in /usr/bin/clickhouse
14. DB::BackgroundSchedulePool::threadFunction() @ 0x134d19b6 in /usr/bin/clickhouse
15. void std::__1::__function::__policy_invoker<void ()>::__call_impl<std::__1::__function::__default_alloc_func<ThreadFromGlobalPool::ThreadFromGlobalPool<DB::BackgroundSchedulePool::BackgroundSchedulePool(unsigned long, unsigned long, char const*)::$_1>(DB::BackgroundSchedulePool::BackgroundSchedulePool(unsigned long, unsigned long, char const*)::$_1&&)::'lambda'(), void ()> >(std::__1::__function::__policy_storage const*) @ 0x134d1fcc in /usr/bin/clickhouse
16. ThreadPoolImpl<std::__1::thread>::worker(std::__1::__list_iterator<std::__1::thread, void*>) @ 0xf448d9d in /usr/bin/clickhouse
17. void* std::__1::__thread_proxy<std::__1::tuple<std::__1::unique_ptr<std::__1::__thread_struct, std::__1::default_delete<std::__1::__thread_struct> >, void ThreadPoolImpl<std::__1::thread>::scheduleImpl<void>(std::__1::function<void ()>, int, std::__1::optional<unsigned long>)::'lambda0'()> >(void*) @ 0xf44aece in /usr/bin/clickhouse
18. ? @ 0x7fc3633ff4aa in ?
19. __clone @ 0x7fc36332e8c3 in ?
 (version 22.8.9.1)
```


zk 重新初始化 ch session 期间 ch table会以 is_readonly 模式打开
[官方文档说明](https://clickhouse.com/docs/en/operations/system-tables/replicas)
```
is_readonly (UInt8) - Whether the replica is in read-only mode. 
This mode is turned on if the config does not have sections 
with ClickHouse Keeper, if an unknown error occurred 
when reinitializing sessions in ClickHouse Keeper, 
and during session reinitialization in ClickHouse Keeper.
```
查看只读的table:
```SQL
SELECT *
FROM system.replicas
WHERE is_readonly = 1;
```

```
{} <Warning> Application: Integrity check of the executable skipped 
because the reference checksum could not be read. 
(calculated checksum: 44DC1265BF0707C939EB10F0AF9FA11E)

{} <Error> Application: DB::Exception: Cannot lock file 
/data/clickhouse/clickhouse-server/status. Another server instance 
in same directory is already running.
```
因为某些原因导致同一服务器上启动两次clickhouse进程触发（如：zk链接丢失 重新初始化session）