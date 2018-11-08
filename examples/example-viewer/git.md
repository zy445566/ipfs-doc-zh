# 使Git更加分布式
你有没有对自己说：“伙计，我的git服务器分布不够”或“我希望我有一种简单的方法可以在全球范围内提供静态git存储库”。祝福不再，我有解决方案给你！

在本文中，我将讨论如何通过ipfs网络提供git存储库。最终结果将是一个git clone通过ipfs服务的能力网址！

首先，选择要托管的git仓库，并对其进行裸代理：
```sh
$ git clone --bare git@myhost.io/myrepo
```
对于那些不是超级git精明的人来说，一个简单的回购意味着它没有工作树，并且可以用作服务器。它们的格式与普通的git repo略有不同。

现在，为了准备好克隆它，您需要执行以下操作：
```sh
$ cd myrepo
$ git update-server-info
```
或者，您可以解压缩所有gits对象：
```sh
$ cp objects/pack/*.pack .
$ git unpack-objects < ./*.pack
$ rm ./*.pack
```
这样做可以将gits large packfile分解为所有单独的对象。如果您添加此git存储库的多个版本，这将允许ipfs重复删除对象。

一旦你完成了这个，那个回购已经准备好了。剩下要做的就是将它添加到ipfs：
```sh
$ pwd
/code/myrepo
$ ipfs add -r .
...
...
...
added QmX679gmfyaRkKMvPA4WGNWXj9PtpvKWGPgtXaF18etC95 .
```
现在，剩下的就是尝试克隆它：

```sh
$ cd /tmp
$ git clone http://localhost:8080/ipfs/QmX679gmfyaRkKMvPA4WGNWXj9PtpvKWGPgtXaF18etC95 myrepo
```
`注意：请确保更改您的哈希值。`

现在，你可能会问“git repo有什么好处，我无法改变任何东西？” 好吧，让我告诉你一个很棒的用例！我倾向于使用一种名为Go的语言进行编程，对于那些不知道的人来说，使用版本控制路径进行导入，即：
```go
import (
    "github.com/whyrusleeping/mycoollibrary"
)
```
这是一个非常好的功能，并解决了很多问题，但很多时候，我遇到了使用someones库的问题，他们更改了API，它破坏了我的代码。使用我们上面所做的，您可以克隆库，并将其添加到ipfs中，因此您的导入路径现在看起来像：
```go
import (
    mylib "gateway.ipfs.io/ipfs/QmX679gmfyaRkKMvPA4WGNWXj9PtpvKWGPgtXaF18etC95"
)
```
并且每次都保证您拥有相同的代码！

注意：由于go不允许将localhost用于导入路径，因此我们使用公共http网关。这不提供安全保证，因为中间人攻击可能会向您发送错误的代码。您可以使用重定向到localhost的域名来避免此问题。

作者:[whyrusleeping](https://github.com/whyrusleeping)