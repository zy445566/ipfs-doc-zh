# 私有网络集群
允许ipfs仅连接到具有共享密钥的其他对等方。

出处： https://github.com/ipfs/go-ipfs/blob/master/docs/experimental-features.md#private-networks

# 状态
试验性（请勿在生产环境使用，但由于我觉得十分有必要所以翻译了）

# 在版本中
master，0.4.7

# 如何启用
使用ipfs-swarm-key-gen生成预共享密钥：
```sh
# node用户可使用：
# npm install node-ipfs-swarm-key-gen
# node-ipfs-swarm-key-gen > ~/.ipfs/swarm.key
go get github.com/Kubuxu/go-ipfs-swarm-key-gen/ipfs-swarm-key-gen
ipfs-swarm-key-gen > ~/.ipfs/swarm.key
```
要加入给定的专用网络，请从网络中的某个人那里获取密钥文件并将其保存到~/.ipfs/swarm.key（如果您使用的是自定义$IPFS_PATH，请将其放在那里）。

使用此功能时，您将无法连接到默认引导程序节点（因为我们不属于您的专用网络），因此您需要设置自己的引导程序节点。

首先，要防止您的节点甚至尝试连接到默认的引导程序节点，请运行：
```sh
ipfs bootstrap rm --all
```
然后添加自己的bootstrap对等体：
```sh
ipfs bootstrap add < multiaddr >
```
例如：
```sh
ipfs bootstrap add /ip4/104.236.76.40/tcp/4001/ipfs/QmSoLV4Bbm51jM9C4gDYZQ9Cy3U6aXMJDAbzgu2fzaDs64
```
除了它们所服务的功能之外，引导节点与网络中的所有其他节点没有区别。

为了格外小心，您还可以将LIBP2P_FORCE_PNET环境变量设置1为强制使用专用网络。如果未配置专用网络，则守护程序将无法启动。

