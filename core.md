* [获取安装包](#%E8%8E%B7%E5%8F%96%E5%AE%89%E8%A3%85%E5%8C%85)
* [安装部署](#%E5%AE%89%E8%A3%85%E9%83%A8%E7%BD%B2)
* [启动验证](#%E5%90%AF%E5%8A%A8%E9%AA%8C%E8%AF%81)
* [验证是否可用](#%E9%AA%8C%E8%AF%81%E6%98%AF%E5%90%A6%E5%8F%AF%E7%94%A8)
* [节点常用指令](#%E8%8A%82%E7%82%B9%E5%B8%B8%E7%94%A8%E6%8C%87%E4%BB%A4)
* [RPC端口](#rpc%E7%AB%AF%E5%8F%A3)

## 获取安装包
下载mtchaind-0.5.6-ubuntu.tar.gz文件,解压后包含如下文件：
```
mtchaind
mtchain-core.cfg
validators.txt 
libprotobuf.so.8 
start.sh
```

## 安装部署
操作系统：Ubuntu14.04_64、Ubuntu16.04_64 请使用root用户执行，并确认root用户对相应的目录有读写权限。  
部署可执行程序目录
```
mkdir /usr/local/mtchain
mv ./mtchaind /usr/local/mtchain
mv ./start.sh /usr/local/mtchain/start.sh
```

拷贝测试网配置文件到当前目录
```
mv ./mtchain-core.cfg /usr/local/mtchain
mv ./validators.txt /usr/local/mtchain
```

部署依赖库文件目录
```
mv ./libprotobuf.so.8 /usr/lib/x86_64-linux-gnu/libprotobuf.so.8

# 注:当操作系统为Ubuntu18.04_64时，需要额外进行如下步骤
mv ./libssl.so.1.0.0 /usr/lib/x86_64-linux-gnu/libssl.so.1.0.0
mv ./libcrypto.so.1.0.0 /usr/lib/x86_64-linux-gnu/libcrypto.so.1.0.0
```

## 启动验证
首次启动，选用这种模式，会从智能流量链MTChain-Core区块链的其他网络上节点同步初始化历史账本信息。
```
./start.sh
```
执行如下命令，返回响应信息中包含如下形式的其他节点的IP等信息，说明本节点已经联网接入成功。
```
./mtchaind    peers
```
```
{
   "id" : 1,
   "result" : {
      "peers" : [
         {
            "address" : "123.132.131.124:51266",
            "complete_ledgers" : "1 - 7249",
            "latency" : 10,
            "ledger" : "B3E803491BD3E174AFD675B8E918CFDF092655EF79ADFA5101D5556722DFF36C",
            "load" : 25,
            "public_key" : "n9Lgj8n1vCoGNKgk9SDU73cCw4vWqeRsStc9xohbBQGF6bsvojrW",
            "uptime" : 12319,
            "version" : "mtchaind-0.5.6"
         },
         ...,
         ...,
        {
            "address" : "123.152.151.214:55124",
            "complete_ledgers" : "730 - 6879",
            "inbound" : true,
            "latency" : 37,
            "ledger" : "B770853172289E265E30AFE527CE0B9CDBDFFDF13438680E539353AA82D7ED1A",
            "load" : 135,
            "public_key" : "n9J6BDLuSiYXpo6zYQghprVgs5JACW7jUoWLK2FFzDHRBPBzRw41",
            "uptime" : 1967,
            "version" : "mtchaind-0.5.6"
         }
      ],
      "status" : "success"
   }
}
```

## 验证是否可用
新建的节点接入主网后，这个时候节点还不可用。只有当节点和主网握手完成，并开始同步账本之后，才进入可用状态。同步账本从最新的账本，向最旧的账本推进。当节点已经同步了最新的账本，并和主网其他节点保持同步的状态后，该节点才算进入可用状态。即可以对外提供服务。可以通过如下命令判断节点状态。 执行如下命令：
```
./mtchaind    server_info
```
```
{
   "id" : 1,
   "result" : {
      "info" : 
            "build_version": "0.5.6",
            "complete_ledgers": "1-88952",
            "hostid": "DRAW",
            "io_latency_ms": 1,
            "last_close": {
                "converge_time_s": 9.999000000000001,
                "proposers": 0
            },
            "load_factor": 1,
            "peers": 0,
            "pubkey_node": "n9rfjidP5RQ2UrDVt1gsbSSGgGbjZseZ2hBMcPsdCpDa1nDLLTVV",
            "server_state": "proposing",
            "state_accounting": {
                "connected": {
                    "duration_us": "0",
                    "transitions": 0
                },
                "disconnected": {
                    "duration_us": "10096253",
                    "transitions": 1
                },
                "full": {
                    "duration_us": "808665295721",
                    "transitions": 1
                },
                "syncing": {
                    "duration_us": "942",
                    "transitions": 1
                },
                "tracking": {
                    "duration_us": "0",
                    "transitions": 1
                }
            },
            "uptime": 808676,
            "validated_ledger": {
                "age": 7,
                "base_fee_m": 0.0001,
                "hash": "E5C64E31ABDB005F35931B1F9A81A7CC796500B4D2A5C58CA6B1D15357F70072",
                "reserve_base_m": 0.0001,
                "reserve_inc_m": 0.0005,
                "seq": 88952
            },
            "validation_quorum": 1
        },
      "status" : "success"
   }
}
```
判断当前节点是否可用的方法一般如下： 首先，complete_ledgers是一个区间，不能是empty；然后，server_state不能是disconnected，connected，syncing状态；最后，age是上一个被验证的账本到现在的时间间隔，不能超过120s，即不能超过2分钟。

## 节点常用指令
```
Commands: 
     获取账号信息
     account_info <account>|<seed>|<pass_phrase>|<key> [<ledger>] [strict]

     获取账号的信任线
     account_lines <account> <account>|"" [<ledger>]

     获取账号拥有的资产列表
     account_currencies <account> [<ledger>] [strict] 

     获取账号的交易历史
     account_tx accountID [ledger_min [ledger_max [limit [offset]]]] [binary] [count] [descending]

     连接其他节点
     connect <ip> [<port>]

     获取共识信息
     consensus_info

     获取节点服务器统计信息
     get_counts

     获取账本信息
     ledger [<id>|current|closed|validated] [full]

     获取最新关闭账本信息
     ledger_closed

     获取当前账本信息
     ledger_current

     在线链上请求账本信息
     ledger_request <ledger>

     查看和设置节点服务器日志级别
     log_level [[<partition>] <severity>]

     获取和其他节点的连接信息
     peers

     获取节点在线状态
     ping

     生成随机种子
     random

     查询节点版本
     version

     获取节点服务器信息
     server_info

     签名
     sign <private_key> <tx_json> [offline]

     多签
     sign_for <signer_address> <signer_private_key> <tx_json> [offline]

     停止节点服务器
     stop

     提交交易请求
     submit <tx_blob>|[<private_key> <tx_json>]

     提交多签交易请求
     submit_multisigned <tx_json>

     查询交易详情
     tx <id>

     创建验证器公私钥
     validation_create [<seed>|<pass_phrase>|<key>]

     创建智能流量链区块链账号公私钥
     wallet_propose [<passphrase>]
```

## RPC端口
查看“mtchain-core.cfg”中RPC接口的端口号，通过 http://节点IP:RPC端口 的方式，参考JSON-RPC API来使用区块链服务。
