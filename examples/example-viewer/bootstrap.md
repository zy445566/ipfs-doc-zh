# 修改默认连接节点列表
IPFS引导列表是IPFS守护程序与网络上的其他对等方进行了解的对等方列表。IPFS附带一个可信对等体的默认列表，但您可以自由修改列表以满足您的需要。自定义引导列表的一个常用用途是创建个人IPFS网络。

首先，让我们列出节点的引导列表：
```sh
> ipfs bootstrap list
/ip4/104.131.131.82/tcp/4001/ipfs/QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtfsmvsqQLuvuJ
/ip4/104.236.151.122/tcp/4001/ipfs/QmSoLju6m7xTh3DuokvT3886QRYqxAzb1kShaanJgW36yx
/ip4/104.236.176.52/tcp/4001/ipfs/QmSoLnSGccFuZQJzRadHn95W2CrSFmZuTdDWP8HXaHca9z
/ip4/104.236.179.241/tcp/4001/ipfs/QmSoLpPVmHKQ4XTPdz8tjDFgdeRFkpV8JgYq8JVJ69RrZm
/ip4/104.236.76.40/tcp/4001/ipfs/QmSoLV4Bbm51jM9C4gDYZQ9Cy3U6aXMJDAbzgu2fzaDs64
/ip4/128.199.219.111/tcp/4001/ipfs/QmSoLSafTMBsPKadTEgaXctDQVcqN88CNLHXMkTNwMKPnu
/ip4/162.243.248.213/tcp/4001/ipfs/QmSoLueR4xBeUbY9WZ9xGUUxunbKWcrNFTDAadQJmocnWm
/ip4/178.62.158.247/tcp/4001/ipfs/QmSoLer265NRgSp2LA3dPaeykiS1J6DifTC88f5uVQKNAd
/ip4/178.62.61.185/tcp/4001/ipfs/QmSoLMeWqB7YGVLJN3pNLQpmmEk35v6wYtsMGLzSr5QBU3
```
上面列出的行是默认IPFS引导节点的地址 - 它们由IPFS开发团队运行。列出的地址已完全解析并以[multiaddr](https://github.com/jbenet/multiaddr)格式指定，这使得每个协议都是显式的。这样，您的节点就可以准确地知道到达引导程序节点的位置 - 该位置是明确的。

除非您了解这样做的意义，否则请勿更改此列表。Bootstrapping是分布式系统中一个重要的安全故障点：恶意引导对等方只能将您引入其他恶意对等方。建议保留IPFS开发团队提供的默认列表，或者 - 在设置专用网络的情况下 - 保留您控制的节点列表。不要将同行添加到您不信任的此列表中。

在这里，我们将新的对等体添加到引导列表：
```sh
> ipfs bootstrap add /ip4/25.196.147.100/tcp/4001/ipfs/QmaMqSwWShsPg2RbredZtoneFjXhim7AQkqbLxib45Lx4S
```
这里我们从引导列表中删除一个节点：
```sh
> ipfs bootstrap rm /ip4/128.199.219.111/tcp/4001/ipfs/QmSoLSafTMBsPKadTEgaXctDQVcqN88CNLHXMkTNwMKPnu
```
假设我们要创建新引导列表的备份。我们可以通过将stdout重定向ipfs bootstrap list到文件来轻松完成此操作：
```sh
> ipfs bootstrap list >save
```
如果我们想要从头开始，我们可以立即删除整个引导程序列表：
```sh
> ipfs bootstrap rm --all
```
使用空列表，我们可以恢复默认引导列表：
```sh
> ipfs bootstrap add --default
```
再次删除整个引导列表，并通过将保存文件的内容传递到ipfs bootstrap add以下内容来恢复我们保存的列表：
```sh
> ipfs bootstrap rm --all
> cat save | ipfs bootstrap add
```

作者:[jbenet](http://github.com/jbenet)和 [insanity54](http://github.com/insanity54)