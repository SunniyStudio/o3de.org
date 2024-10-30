---
linktitle: 网络和多人设置
title: AzNetworking 和 Multiplayer Gem 设置
description: 控制台变量和其他设置的参考资料，您可以在客户端和服务器上使用这些变量和设置来配置 `AzNetworking`和 Multiplayer Gem。
weight: 300
---

## 概述
本页记录了[控制台变量（CVAR）](/docs/user-guide/appendix/cvars/)和其他设置，这些设置可以控制**Open 3D Engine (O3DE)**中的联网和多人游戏行为。

## 网络命令

使用以下 [控制台函数(cfunc)](/docs/user-guide/programming/az-console/#console-functors-cfuncs) 命令控制客户端与服务器的连接，以进行联网游戏或模拟。

| 设置                     | 说明                                           | 参数                                         | 注意                                    |
|------------------------|----------------------------------------------|--------------------------------------------|---------------------------------------|
| host                   | 作为主机打开多人连接，供其他客户端连接。                         |                                            |                                       |
| connect                | 打开与主机的多人连接。                                  | **(可选)** IP地址和端口，由':'分割。例如，`0.0.0.0:1234`. | 默认值为 `cl_serveraddr`:`cl_serverport`. |
| disconnect             | 断开所有打开的多人游戏连接。                               |                                            |                                       |
| LoadLevel              | 卸载当前关卡，并使用给定的资产名称加载新关卡。 <br> 用于设置游戏或模拟的初始关卡。 | **(必须)** 关卡文件的路径。                          | 指令并非专门针对网络或多人游戏，而是用于所有游戏和模拟。          | 
| sv_launch_local_client | 启动本地客户端并连接到该主机服务器。                           |                                            | 仅在当前主机托管时有效。                          |

这些控制台命令可以通过[控制台命令行](/docs/user-guide/editor/console/)动态执行，也可以放在控制台命令配置文件中。

使用控制台命令配置文件时，通常的做法是将文件放在项目根目录下。使用 `.cfg` 作为文件名后缀。命令将按照写入的顺序执行。

对于联网游戏或模拟游戏，典型的 **服务器** 配置文件应包含以下内容：

```cmd
host
LoadLevel <path to Level>
```

典型的**客户端**配置文件应包含

```cmd
connect
```

可以使用 `console-command-file` 选项将配置文件中的控制台命令传递给客户端和服务器启动器。例如：

```cmd
MultiplayerSample.ServerLauncher.exe --console-command-file=launch_server.cfg
```

## 客户端设置
`cl` CVARs 控制客户端行为。

| 设置                     | 说明                                                     | 默认值       | 注意 |
|------------------------|--------------------------------------------------------|-----------|----|
| cl_clientport          | 连接远程主机时游戏流量绑定的端口，值为 0 时将选择任何可用端口。                      | 0         |    |
| cl_serveraddr          | 要连接的远程服务器或主机的地址。                                       | LocalHost |    |
| cl_serverport          | 用于连接游戏流量的远程主机端口。                                       | 33450     | 
| cl_InputRateMs         | 采样和处理客户输入的速度。                                          | 33 ms     |    |
| cl_MaxRewindHistoryMs  | 为服务器修正倒带和重放保留的最大毫秒数。                                   | 2000 ms   |
| cl_renderTickBlendBase | 用于网络更新之间混合的基数，0.1 会比较线性，0.2 或 0.3 则会更慢，可能更适合延迟变化很大的连接。 | 0.15      |    |

### 客户端到服务器的连接设置

| 设置                                             | 说明                                    | 默认值        | 注意 |
|------------------------------------------------|---------------------------------------|------------|----|
| cl_ClientEntityReplicatorPendingRemovalTimeMs  | 通过更改复制窗口为客户端删除实体之前需要等待多长时间，实体删除仍是即时的。 | 10000 ms   |    |
| cl_DefaultNetworkEntityActivationTimeSliceMs   | 用于激活来自网络的实体的最大 Ms，0 表示实例化所有内容。        | 0 ms       |    |
| cl_ClientMaxRemoteEntitiesPendingCreationCount | 我们已发送给客户但尚未得到客户确认的实体的最大数量。            | 4294967295 |    |

### 调试设置

| 设置                             | 说明                                   | 默认值   | 注意     |
|--------------------------------|--------------------------------------|-------|--------|
| cl_DebugHackTimeMultiplier     | 用于模拟时钟黑客作弊的标量值，以验证银行时间系统和 anticheat。 | 1.0   | 非发布版本  |
| cl_EnableDesyncDebugging       | 如果启用，调试日志将包含检测到的状态不同步的详尽信息。          | True  | 非发布版本  |
| cl_DesyncDebugging_AuditInputs | 如果为true，则将输入添加到审计跟踪中。                | False | 非发布版本  |
| cl_PredictiveStateHistorySize  | 控制应保留多少个预测状态输入，以调试脱同步。               | 120   | 仅非发布版本 |


## 服务器设置

`sv` CVARs控制服务器行为。

| 设置                                | 说明                                                    | 默认值   | 注意   |
|-----------------------------------|-------------------------------------------------------|-------|------|
| sv_map                            | 服务器应该加载的地图。                                           | None  |      |
| sv_port                           | Multiplayer Gem 用于绑定游戏流量的端口。                          | 33450 |      |
| sv_portRange                      | 主机初始化时将尝试绑定的增量端口范围。                                   | 999   |      |
| sv_protocol                       | 该标志控制我们使用 TCP 还是 UDP 进行游戏联网。                          | Udp   |      |
| sv_DedicatedCPUPercent            | 作为专用服务器运行时的目标 CPU 占用率。                                | 0     | 默认禁用 |
| sv_DedicatedCPUVariance           | CPU 与 `sv_DedicatedCPUPercent` 的差值。                   | 10    |      |
| sv_DedicatedMaxRate               | 作为专用服务器运行时的最大更新率。                                     | 30    |      |
| sv_isDedicated                    | 主机命令是创建独立服务器还是客户端托管服务器。                               | True  |      |
| sv_isTransient                    | 如果所有现有连接都断开，专用服务器是否会关闭。                               | True  |      |
| sv_serverSendRateMs               | 每次网络更新之间的最小间隔毫秒数。                                     | 50 ms |      | 
| sv_versionMismatch_autoDisconnect | 决定不匹配的连接是否会自动终止。建议保持此值为真；即使是多人游戏组件版本之间的微小差异也可能导致意外行为。 | True  |      |

### 服务器到客户端连接设置
| 设置                                                     | 说明                                    | 默认值        | 注意 |
|--------------------------------------------------------|---------------------------------------|------------|----|
| sv_ClientEntityReplicatorPendingRemovalTimeMs          | 通过更改复制窗口为客户端删除实体之前需要等待多长时间，实体删除仍是即时的。 | 10000 ms   |    |
| sv_ClientMaxRemoteEntitiesPendingCreationCount         | 我们已发送给客户端但尚未得到客户端确认的实体的最大数量。          | 4294967295 |    |
| sv_ClientMaxRemoteEntitiesPendingCreationCountPostInit | 游戏开始后，我们将发送给客户的实体的最大数量。               | 4294967295 |    |

### Replication window settings
| 设置                                 | 说明                         | 默认值    | 注意 |
|------------------------------------|----------------------------|--------|----|
| sv_ReplicateServerProxies          | 启用向客户端发送 ServerProxy 实体。   | True   |    |
| sv_MaxEntitiesToTrackReplication   | 复制时要跟踪的实体的默认最大数量。          | 512    |    |
| sv_MinEntitiesToReplicate          | 向客户端连接复制实体的默认最少数量。         | 128    |    |
| sv_MaxEntitiesToReplicate          | 向客户端连接复制实体的默认最大数量。         | 256    |    |
| sv_PacketsToIntegrateQos           | 更新连接服务质量指标前累计的数据包数量。       | 1000   |    |
| sv_BadConnectionThreshold          | 损失百分比，超过这个百分比，我们就认为网络出现问题。 | 0.25   |    |
| sv_ClientReplicationWindowUpdateMs | 复制窗口更新速率。                  | 300 ms |    |               
| sv_ClientAwarenessRadius           | 实体与客户端的最大距离，但仍具有相关性。       | 500.0  |    |

### Rewind 设置
| 设置                             | 说明                      | 默认值 | 注意 |
|--------------------------------|-------------------------|-----|----|
| sv_RewindVolumeExtrudeDistance | 增加倒带量检查的数量，以考虑到快速移动的实体。 | 50  |    |

### 编辑器服务器设置
`editorsv` 设置可控制服务器在 “编辑器播放”（CTRL+G）模式下的启动方式。

| 设置                                 | 说明                                                                                                                                                            | 默认值       | 注意                                                                                           |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------|----------------------------------------------------------------------------------------------|
| editorsv_isDedicated               | 是否作为服务器从编辑器获取数据。除非您确信自己在做什么，否则不要修改。                                                                                                                           | False     |                                                                                              |
| editorsv_port                      | 编辑器进入播放模式时将连接的服务器端口。                                                                                                                                          | 33450     | 仅当 `editorsv_enabled` 为 true 时有效。                                                            | 
| editorsv_serveraddr                | 编辑器进入播放模式时将连接的服务器地址。                                                                                                                                          | LocalHost | 仅当 `editorsv_enabled` 为 true 时有效。                                                            |
| editorsv_process                   | 项目服务器启动器的文件路径。如果 `editorsv_enabled` 和 `editorsv_launch` 为 true，编辑器将尝试在进入游戏模式时启动自己的服务器。默认情况下，它会在编辑器所在的同一文件夹中查找 ServerLauncher 可执行文件；`editorsv_process` 会覆盖该路径。 | ""        |                                                                                              |
| editorsv_rhi_override              | 如果 `editorsv_launch` 为 true，服务器将使用与编辑器相同的渲染硬件接口 (rhi)。例如，如果编辑器使用的是 DX12，那么新服务器将使用 DX12 启动。可使用 `editorsv_rhi_override` 来覆盖 Rhi。                                | ""        | 如果不需要查看已启动服务器的图形，则设置 `editorsv_rhi_override=null` 即空渲染器。                                     |
| editorsv_enabled                   | 如果为 true，编辑器将尝试在进入游戏模式后连接到多人游戏服务器。                                                                                                                            | False     |                                                                                              |
| editorsv_clientserver              | 编辑器是否应同时充当服务器和客户端，而无需启动专门的服务器进程。                                                                                                                              | False     | 仅当 `editorsv_enabled` 也为 true 时适用。                                                           |
| editorsv_hidden                    | 如果设置为 true，编辑器将在进入游戏模式后自动启动服务器。如果希望启动的服务器作为后台进程启动，而没有任何可见窗口，请将 `editorsv_hidden` 设为 true。                                                                     | False     |                                                                                              |  
| editorsv_launch                    | 如果为 “true”，编辑器将在进入游戏模式后自动启动自己的服务器。                                                                                                                            | True      | 只适用于 `editorsv_enabled` 也为 true 的情况。如果启动自己的编辑器服务器，请记住在服务器上将 `editorsv_isDedicated` 设置为 true。 |
| editorsv_connectionMessageFontSize |                                                                                                                                                               | 0.7       |                                                                                              |


### 双客户端服务器设置
`bg`设置可控制客户端和服务器上的行为。

| 设置                                                  | 说明                                       | 默认值   | 注意 |
|-----------------------------------------------------|------------------------------------------|-------|----|
| bg_RewindDebugDraw                                  | 如果为 true，则启用倒带操作的调试绘制。                   | False |    |
| bg_replicationWindowImmediateAddRemove              | 在可见性添加/删除时立即更新复制窗口。                      | True  |    |
| bg_multiplayerDebugDraw                             | 启用Multiplayer Gem的调试绘制。                  | False |    |
| bg_replicationWindowImmediateAddRemove              | 在可见性添加/删除时立即更新复制窗口。                      | True  |    |
| bg_RewindPositionTolerance                          | 如果 delta 位置的平方小于此值，则不同步 NVIDIA PhysX 实体。 | 0.001 |    |
| bg_AssertNetBindOnDeactivationWithoutMarkForRemoval |                                          | False |    |
| bg_hierarchyEntityMaxLimit                          |                                          | 16    |    | 
| bg_DrawArticulatedHitVolumes                        | 启用调试绘制衔接命中体积。                            | False |    |
| bg_DrawDebugHitVolumeLifetime                       | 命中体积绘制调试形状的寿命。                           | 0.0   |    |
| bg_RewindOrientationTolerance                       | 如果 delta 方向的平方小于此值，则不同步 NVIDIA PhysX 实体。 | 0.001 |    |


## 网络设置
`net` 设置控制网络行为。

### AzNetworking 层

| 设置                             | 说明                                                       | 默认值             | 注意              |
|--------------------------------|----------------------------------------------------------|-----------------|-----------------|
| net_maxPacketTrackTimeMs       | 在放弃之前跟踪任何特定数据包 ID 的最长时间。                                 | 2000            |                 |
| net_rttIncreaseOnPacketLoss    | 因数据包丢失而增加往返时间估计值的标量。                                     | 1.2             |                 |
| net_TcpCompressor              | 要使用的 TCP 压缩器。警告：与加密类似，在创建网络接口前也需要设置一次。                   | None            |                 |
| net_TcpUseEncryption           | 在基于 TCP 的连接上启用加密。                                        | False           |                 |
| net_UseDtlsCookies             | 在连接握手过程中启用 DTLS cookie 交换。                               | False           |                 |
| net_UdpMaxUnackedPacketCount   | 强制接收心跳数据包之前接收的最大数据包数量。                                   | 10              |                 |
| net_UdpFragmentTimeoutMs       | 毫秒，用于在定时结束前保留不完整、不可靠的碎片数据包块。                             | 5000 ms         |                 |
| net_UdpCompressor              | 要使用的 UDP 压缩器。警告：与加密类似，在创建网络接口前也需要设置一次。                   | None            |                 |
| net_MinPacketTimeoutMs         | 未打包数据包超时前的最短等待时间。                                        | 200 ms          |                 |
| net_UdpDefaultTimeoutMs        | 空闲 UDP 连接超时前的时间（毫秒）。                                     | 10 * 1000 ms    |                 |
| net_UdpPacketTimeSliceMs       | 允许处理数据包的毫秒数。                                             | 8 ms            |                 |
| net_UdpTimeoutConnections      | 代码应保持超时 UDP 连接。                                          | True            |                 |
| net_UdpUseEncryption           | 对基于 UDP 的连接启用加密。                                         | False           |                 |
| net_RttFudgeScalar             | 将计算出的 RTT 乘以标量值，以确定最佳数据包超时阈值。                            | 2.0             |                 |
| net_MaxTimeoutsPerFrame        | 允许在单个帧中处理的最大数据包超时数。                                      | 1000            |                 |
| net_FragmentedHeaderOverhead   | 从分片数据包有效载荷中提取的虚拟开销值。                                     | 32              |                 |
| net_SslInflationOverhead       | 从碎片数据包有效载荷中提取的 SSL 虚拟开销值。                                | 32              |                 |
| net_UdpUnackedHeartbeats       | 在放弃连接之前，为保持连接活力而尝试发送的心跳次数。                               | 5               |                 |
| net_UdpMaxReadTimeMs           | 允许阅读器线程从注册套接字读取数据的时间。                                    | 10 ms           |                 |
| net_MaxReliablePacketsInWindow | 在触发断开连接之前允许排队的可靠数据包的最大数量。                                | 16384           |                 |
| net_UdpIgnoreWin10054          | 如果为 true，将忽略 windows 上的 10054 套接字错误。                     | True            |                 |
| net_UdpRecvBufferSize          | 默认 UDP 套接字接收缓冲区大小。                                       | 1 * 1024 * 1024 |                 |
| net_UdpSendBufferSize          | 默认 UDP 套接字发送缓冲区大小。                                       | 1 * 1024 * 1024 |                 |
| net_validateSerializedTypes    | 当尝试 `TypeValidatingSerializer` 序列化导致类型或变量名不匹配时引发 Assert。 | False           | 提高带宽利用率，支持错配检测。 |

### 多人网络层

| 设置                                  | 说明                      | 默认值     | 注意                 |
|-------------------------------------|-------------------------|---------|--------------------|
| net_EntityReplicatorRecordsMax      | 允许的未完成实体记录数。            | 45      | 需要 Multiplayer Gem |
| net_DefaultEntityMigrationTimeoutMs | 在删除实体之前，需要等待新的授权附加到实体上。 | 1000 ms | 需要 Multiplayer Gem |
| net_EntityReplicatorRecordsMax      | 允许的未完成实体记录数。            | 45      | 需要 Multiplayer Gem |
| net_DebugEntities_ShowBandwidth     | 显示带宽信息。                 | False   |                    |
| net_DebutAuditTrail_HistorySize     | 审计历史记录的大小。              | 20      |                    |
| net_DebugEntities_WarnAboveKbps     |                         | 10.f    |                    |
| net_DebugEntities_BelowWarningColor |                         | Grey    |                    |
| net_DebugEntities_WarningColor      |                         | Red     |                    |
| net_DebugEntities_ShowAboveKbps     |                         | 1.f     |                    |



### TLS/DTLS 证书设置
| 设置                             | 说明                                   | 默认值                         | 注意 |
|--------------------------------|--------------------------------------|-----------------------------|----|
| net_SslCertCiphers             | 使用基于证书的密钥交换时要使用的密码套件。                | ECDHE-RSA-AES256-GCM-SHA384 |    |
| net_SslExternalCertificateFile | 外部（服务器到客户端）证书链的 PEM 格式文件名。           | ""                          |    |
| net_SslExternalContextPassword | 外部（服务器到客户端）私人证书所需的密码。                | ""                          |    |
| net_SslExternalPrivateKeyFile  | PEM 格式的外部（服务器到客户端）私钥文件的文件名。          | ""                          |    |
| net_SslInternalCertificateFile | PEM 格式的 内部（仅限服务器到服务器）证书链文件名。         | ""                          |    |
| net_SslInternalContextPassword | 内部（仅限服务器到服务器）私人证书所需的密码。              | ""                          |    |
| net_SslInternalPrivateKeyFile  | PEM 格式的 内部（仅限服务器到服务器）私钥文件的文件名。       | ""                          |    |
| net_RotateCookieTimer          | 在生成新的 DTLS cookie 进行握手前等待的毫秒数。       | 50 ms                       |    |
| net_SslAllowSelfSigned         | 如果启用，自签名证书在其他方面被认为是可信的，则不会导致验证错误。    | True                        |    |
| net_SslEnablePinning           | 如果启用，将对本地端点和远程端点的公共证书进行比较，以确保它们完全匹配。 | True                        |    |
| net_SslValidateExpiry          | 如果启用，将检查证书上的过期日期是否有效。                | True                        |    |
| net_SslMaxCertDepth            | 证书链验证允许的最大深度。                        | 3                           |    |

### 调试设置
| 设置                                      | 说明                                                                                                                      | 默认值   | 注意                 |
|-----------------------------------------|-------------------------------------------------------------------------------------------------------------------------|-------|--------------------|
| net_DebugCheckNetworkEntityManager      | 启用 NetworkEntityManager 内部的额外调试检查。                                                                                      | False | 需要 Multiplayer Gem |
| sv_versionMismatch_sendManifestToClient | 决定当出现不匹配时，服务器是否会向客户端发送所有单个多人游戏组件的版本信息。收到信息后，客户端会在控制台打印出不同的组件。建议在发布版本时设置为 false。这样做是为了防止客户端获取任何应保密的多人游戏组件信息（组件名称和版本哈希值）。 | True  |                    |

### 其他有用的设置
以下设置可作为命令行参数传递，以控制服务器性能。

| 设置           | 说明                                                      |
|--------------|---------------------------------------------------------|
| rhi          | 在服务器启动器参数中添加 `-rhi=null`，打开空渲染器，以避免渲染成本。                |
| NullRenderer | 在服务器启动器参数中添加 `-NullRenderer` 以关闭 RPI 勾选，从而在使用空呈现器时节省性能。 |
