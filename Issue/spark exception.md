에러가 발생할 때 자주보던 에러 로그들 리스트  
잘 봐두면 spark 정리할 때 각 명칭들이 좀 더 눈에 들어올 것이다..  

<br/><br/>

```bash
2021:12:21 07:24:57.867 ERROR [shuffle-client-4-1] org.apache.spark.network.server.TransportChannelHandler: Connection to ihmigration02gm.gmarket.nh/172.30.239.158:27585 has been quiet for 600000 ms while there are outstanding requests. Assuming connection is dead; please adjust spark.network.timeout if this is wrong.

...

2021:12:21 07:24:58.358 WARN  [netty-rpc-env-timeout] org.apache.spark.rpc.netty.NettyRpcEnv: Ignored failure: java.util.concurrent.TimeoutException: Cannot receive any reply from ihmigration02gm.gmarket.nh:18964 in 10000 milliseconds

...

2021:12:21 07:24:59.056 WARN  [shuffle-server-5-2] org.apache.spark.network.server.TransportChannelHandler: Exception in connection from /172.30.239.158:43865
java.io.IOException: Connection reset by peer
        at sun.nio.ch.FileDispatcherImpl.read0(Native Method)
        at sun.nio.ch.SocketDispatcher.read(SocketDispatcher.java:39)
        at sun.nio.ch.IOUtil.readIntoNativeBuffer(IOUtil.java:223)
        at sun.nio.ch.IOUtil.read(IOUtil.java:192)
        at sun.nio.ch.SocketChannelImpl.read(SocketChannelImpl.java:380)
        at io.netty.buffer.PooledByteBuf.setBytes(PooledByteBuf.java:247)
        at io.netty.buffer.AbstractByteBuf.writeBytes(AbstractByteBuf.java:1140)
        at io.netty.channel.socket.nio.NioSocketChannel.doReadBytes(NioSocketChannel.java:347)
        at io.netty.channel.nio.AbstractNioByteChannel$NioByteUnsafe.read(AbstractNioByteChannel.java:148)
        at io.netty.channel.nio.NioEventLoop.processSelectedKey(NioEventLoop.java:697)
        at io.netty.channel.nio.NioEventLoop.processSelectedKeysOptimized(NioEventLoop.java:632)
        at io.netty.channel.nio.NioEventLoop.processSelectedKeys(NioEventLoop.java:549)
        at io.netty.channel.nio.NioEventLoop.run(NioEventLoop.java:511)
        at io.netty.util.concurrent.SingleThreadEventExecutor$5.run(SingleThreadEventExecutor.java:918)
        at io.netty.util.internal.ThreadExecutorMap$2.run(ThreadExecutorMap.java:74)
        at io.netty.util.concurrent.FastThreadLocalRunnable.run(FastThreadLocalRunnable.java:30)
        at java.lang.Thread.run(Thread.java:748)

...

2021:12:21 07:25:00.743 ERROR [shuffle-client-4-1] org.apache.spark.network.client.TransportResponseHandler: Still have 1 requests outstanding when connection from ihmigration02gm.gmarket.nh/172.30.239.158:27585 is closed

...

2021:12:21 07:25:08.115 INFO  [ForkJoinPool.commonPool-worker-18] com.ebaykorea.baobab.porter.service.SyncerService: Upload : mg/d/a/day2tem/intro_mantoman/INTRO2_2.jpg

```

```bash
2021:12:21 22:00:23.432 WARN  [driver-heartbeater] org.apache.spark.executor.Executor: Issue communicating with driver in heartbeater
org.apache.spark.rpc.RpcTimeoutException: Futures timed out after [10000 milliseconds]. This timeout is controlled by spark.executor.heartbeatInterval
        at org.apache.spark.rpc.RpcTimeout.org$apache$spark$rpc$RpcTimeout$$createRpcTimeoutException(RpcTimeout.scala:47)
        at org.apache.spark.rpc.RpcTimeout$$anonfun$addMessageIfTimeout$1.applyOrElse(RpcTimeout.scala:62)
        at org.apache.spark.rpc.RpcTimeout$$anonfun$addMessageIfTimeout$1.applyOrElse(RpcTimeout.scala:58)
        at scala.runtime.AbstractPartialFunction.apply(AbstractPartialFunction.scala:38)
        at org.apache.spark.rpc.RpcTimeout.awaitResult(RpcTimeout.scala:76)
        at org.apache.spark.rpc.RpcEndpointRef.askSync(RpcEndpointRef.scala:92)
        at org.apache.spark.executor.Executor.org$apache$spark$executor$Executor$$reportHeartBeat(Executor.scala:841)
        at org.apache.spark.executor.Executor$$anon$2.$anonfun$run$24(Executor.scala:870)
        at scala.runtime.java8.JFunction0$mcV$sp.apply(JFunction0$mcV$sp.java:23)
        at org.apache.spark.util.Utils$.logUncaughtExceptions(Utils.scala:1945)
        at org.apache.spark.executor.Executor$$anon$2.run(Executor.scala:870)
        at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
        at java.util.concurrent.FutureTask.runAndReset(FutureTask.java:308)
        at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.access$301(ScheduledThreadPoolExecutor.java:180)
        at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:294)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
        at java.lang.Thread.run(Thread.java:748)
Caused by: java.util.concurrent.TimeoutException: Futures timed out after [10000 milliseconds]
at scala.concurrent.impl.Promise$DefaultPromise.ready(Promise.scala:259)
at scala.concurrent.impl.Promise$DefaultPromise.result(Promise.scala:263)
at org.apache.spark.util.ThreadUtils$.awaitResult(ThreadUtils.scala:220)
at org.apache.spark.rpc.RpcTimeout.awaitResult(RpcTimeout.scala:75)
... 13 common frames omitted

...

2021:12:21 22:00:40.592 WARN [dispatcher-event-loop-14] org.apache.spark.HeartbeatReceiver: Removing executor driver with no recent heartbeats: 733973 ms exceeds timeout 600000 ms
2021:12:21 22:00:40.767 ERROR [dispatcher-event-loop-14] org.apache.spark.scheduler.TaskSchedulerImpl: Lost executor driver on localhost: Executor heartbeat timed out after 733973 ms
2021:12:21 22:00:41.627 WARN [dispatcher-event-loop-14] org.apache.spark.scheduler.TaskSetManager: Lost task 4.0 in stage 139138.0 (TID 297673, localhost, executor driver): ExecutorLostFailure (executor driver exited caused by one of the running tasks) Reason: Executor heartbeat timed out after 733973 ms
2021:12:21 22:00:41.655 ERROR [dispatcher-event-loop-14] org.apache.spark.scheduler.TaskSetManager: Task 4 in stage 139138.0 failed 1 times; aborting job

...

2021:12:21 22:00:41.852 WARN [dispatcher-event-loop-14] org.apache.spark.rpc.netty.NettyRpcEnv: Ignored message: HeartbeatResponse(true)

...

org.apache.spark.SparkException: Job aborted due to stage failure: Task 4 in stage 139138.0 failed 1 times, most recent failure: Lost task 4.0 in stage 139138.0 (TID 297673, localhost, executor driver): ExecutorLostFailure (executor driver exited caused by one of the running tasks) Reason: Executor heartbeat timed out after 733973 ms
Driver stacktrace:
at org.apache.spark.scheduler.DAGScheduler.failJobAndIndependentStages(DAGScheduler.scala:1889)
at org.apache.spark.scheduler.DAGScheduler.$anonfun$abortStage$2(DAGScheduler.scala:1877)
at org.apache.spark.scheduler.DAGScheduler.$anonfun$abortStage$2$adapted(DAGScheduler.scala:1876)
at scala.collection.mutable.ResizableArray.foreach(ResizableArray.scala:62)
at scala.collection.mutable.ResizableArray.foreach$(ResizableArray.scala:55)
at scala.collection.mutable.ArrayBuffer.foreach(ArrayBuffer.scala:49)
at org.apache.spark.scheduler.DAGScheduler.abortStage(DAGScheduler.scala:1876)
at org.apache.spark.scheduler.DAGScheduler.$anonfun$handleTaskSetFailed$1(DAGScheduler.scala:926)
at org.apache.spark.scheduler.DAGScheduler.$anonfun$handleTaskSetFailed$1$adapted(DAGScheduler.scala:926)
at scala.Option.foreach(Option.scala:274)
at org.apache.spark.scheduler.DAGScheduler.handleTaskSetFailed(DAGScheduler.scala:926)
at org.apache.spark.scheduler.DAGSchedulerEventProcessLoop.doOnReceive(DAGScheduler.scala:2110)
at org.apache.spark.scheduler.DAGSchedulerEventProcessLoop.onReceive(DAGScheduler.scala:2059)
at org.apache.spark.scheduler.DAGSchedulerEventProcessLoop.onReceive(DAGScheduler.scala:2048)
at org.apache.spark.util.EventLoop$$anon$1.run(EventLoop.scala:49)
at org.apache.spark.scheduler.DAGScheduler.runJob(DAGScheduler.scala:737)
at org.apache.spark.SparkContext.runJob(SparkContext.scala:2061)
at org.apache.spark.SparkContext.runJob(SparkContext.scala:2082)
at org.apache.spark.SparkContext.runJob(SparkContext.scala:2101)
at org.apache.spark.SparkContext.runJob(SparkContext.scala:2126)
at org.apache.spark.rdd.RDD.$anonfun$collect$1(RDD.scala:945)
at org.apache.spark.rdd.RDDOperationScope$.withScope(RDDOperationScope.scala:151)
at org.apache.spark.rdd.RDDOperationScope$.withScope(RDDOperationScope.scala:112)
at org.apache.spark.rdd.RDD.withScope(RDD.scala:363)
at org.apache.spark.rdd.RDD.collect(RDD.scala:944)
at org.apache.spark.api.java.JavaRDDLike.collect(JavaRDDLike.scala:361)
at org.apache.spark.api.java.JavaRDDLike.collect$(JavaRDDLike.scala:360)
at org.apache.spark.api.java.AbstractJavaRDDLike.collect(JavaRDDLike.scala:45)
at com.ebaykorea.baobab.porter.job.DetectItemProcessor.process(DetectItemProcessor.java:87)
at com.ebaykorea.baobab.porter.job.DetectItemProcessor.process(DetectItemProcessor.java:19)
at sun.reflect.GeneratedMethodAccessor48.invoke(Unknown Source)
at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
at java.lang.reflect.Method.invoke(Method.java:498)
at org.springframework.aop.support.AopUtils.invokeJoinpointUsingReflection(AopUtils.java:343)
at org.springframework.aop.framework.ReflectiveMethodInvocation.invokeJoinpoint(ReflectiveMethodInvocation.java:198)
at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:163)
at org.springframework.aop.support.DelegatingIntroductionInterceptor.doProceed(DelegatingIntroductionInterceptor.java:136)
at org.springframework.aop.support.DelegatingIntroductionInterceptor.invoke(DelegatingIntroductionInterceptor.java:124)
at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:186)
at org.springframework.aop.framework.JdkDynamicAopProxy.invoke(JdkDynamicAopProxy.java:212)
at com.sun.proxy.$Proxy69.process(Unknown Source)

...

2021:12:21 22:00:45.249 WARN  [dispatcher-event-loop-16] org.apache.spark.storage.BlockManagerMasterEndpoint: No more replicas available for taskresult_297669 !
2021:12:21 22:00:45.249 WARN  [dispatcher-event-loop-16] org.apache.spark.storage.BlockManagerMasterEndpoint: No more replicas available for broadcast_231919_piece0 !

```

종종 logs 에 아무것도 기록되지 않고 갑자기 프로세스가 종료되기도 하는데  
이때는 시스템 로그 확인을 해보자: /var/log/messages  


아래 메시지가 있다면 메모리 부족으로 종료되는 것이니,  
spark 의 core 개수나 용량등을 서버 상황에 맞게 조율하는 것이 필요하다. 
Out of memory: Kill process 6148 (java) score 283 or sacrifice child

