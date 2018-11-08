# IPFS的网站
在ipfs上托管您的网站的简短指南
将静态网站添加到ipfs非常简单！只需打开你的守护进程：
```sh
$ ipfs daemon
```
并添加包含您网站的目录：
```sh
$ ls mysite
img index.html
$ ipfs add -r mysite
added QmcMN2wqoun88SVF5own7D5LUpnHwDA6ALZnVdFXhnYhAs mysite/img/spacecat.jpg
added QmS8tC5NJqajBB5qFhcA1auav14iHMnoMZJWfmr4k3EY6w mysite/img
added QmYh6HbZhHABQXrkQZ4aRRSoSa6bb9vaKoHeumWex6HRsT mysite/index.html
added QmYeAiiK1UfB8MGLRefok1N7vBTyX8hGPuMXZ4Xq1DPyt7 mysite/
```
文件夹名称旁边的最后一个哈希是你想要的那个，让我们暂时调用它 $SITE_HASH。

现在，您可以通过 在Web浏览器中打开来在本地测试它！接下来，要查看它来自另一个ipfs节点，您可以尝试 。很酷，对吗？但那些哈希相当难看。让我们看看摆脱它们的一些方法。`http://localhost:8080/ipfs/$SITE_HASH` `http://gateway.ipfs.io/ipfs/$SITE_HASH`

首先，您可以执行包含的简单DNS TXT记录。一旦该记录传播，您应该能够在以下位置查看您的网站 。现在那个更清洁了。你也可以在网关上试试这个`dnslink=/ipfs/$SITE_HASH` `http://localhost:8080/ipns/your.domain` `http://gateway.ipfs.io/ipns/your.domain`

接下来，您可能会问“如果我想要更改我的网站，DNS会很慢！” 那么让我告诉你这个叫做Ipns的小东西（注意'n'）。Ipns是行星际命名系统，您可能已经注意到上面的链接 /ipns/而不是/ipfs/。Ipns用于ipfs网络中的可变内容，它相对容易使用，并且允许您每次更新网站而不更新dns记录！那么你如何使用它？

添加网页后，只需执行以下操作：
```sh
$ ipfs name publish $SITE_HASH
Published to <your peer id>: /ipfs/$SITE_HASH
```
（免责声明：当使用IPNS更新网站时，重要的是要考虑在更新您的网站时可以从两个不同的已解决的哈希值加载资产，导致过时/丢失资产，除非考虑到）

现在，您可以通过查看来测试它的工作原理：。并且还尝试在公共网关上使用相同的链接。一旦你确信它有效，让我们再次隐藏哈希值。将您的DNS TXT记录更改为，等待该记录传播，然后尝试访问。`http://localhost:8080/ipns/<your peer id>` `dnslink=/ipns/<your peer id>` `http://localhost:8080/ipns/your.domain`

此时，您有一个关于ipfs / ipns的网站，您可能想知道如何将其公开，以便今天的互联网用户也可以访问它，而无需了解任何此类信息。这实际上非常简单，您需要的只是先前创建的TXT记录，并将A记录指向ipfs守护程序的IP地址，该守护程序在端口80上侦听HTTP请求（例如 ）。用户的浏览器将发送请求的主机头，并且您有dnslink TXT记录，因此ipfs网关将识别为IPNS名称，因此它将从下而不是。`http://your.domainyour.domaingateway.ipfs.ioyour.domainyour.domain/ipns/your.domain/` `/`

因此，如果您将A记录指向IP ，然后等待DNS传播，那么任何人都应该能够访问您的ipfs托管站点而无需任何额外配置 。`your.domaingateway.ipfs.io` `http://your.domain`

或者，可以使用CNAME记录指向网关的DNS记录。这样，网关的IP地址就会自动更新。但请注意，CNAME记录不允许其他记录，例如TXT引用ipfs / ipns记录。正因为如此，IPF问题可以创建一个DNS TXT记录用 。`_dnslink.your.domain` `dnslink=/ipns/<yourpeer id>`

因此，通过创建CNAME 到并添加 记录与您可以承载你的网站没有明确提及森林小组网关的IP地址。`your.domaingateway.ipfs.io` `_dnslink.your.domain` `dnslink=/ipns/<your peer id>`

黑客快乐！
作者:[whyrusleeping](https://github.com/whyrusleeping)