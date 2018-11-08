# 成为集群
IPFS就是关于网络！包括一组有用的命令，以帮助观察网络。

查看您与谁直接相关联的人：
```sh
ipfs swarm peers
```
手动连接到特定对等方。如果下面的对等点不起作用，请从输出中选择一个ipfs swarm peers。
```sh
ipfs swarm connect /ip4/104.236.176.52/tcp/4001/ipfs/QmSoLnSGccFuZQJzRadHn95W2CrSFmZuTdDWP8HXaHca9z
```
在网络上搜索给定的对等方：
```sh
ipfs dht findpeer QmSoLnSGccFuZQJzRadHn95W2CrSFmZuTdDWP8HXaHca9z
```
作者:[whyrusleeping](https://github.com/whyrusleeping)