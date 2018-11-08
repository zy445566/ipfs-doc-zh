# 行星间命名系统
ipns是一种向ipfs永久不变性添加少量可变性的方法。它允许您在peerID的名称空间（公钥的哈希）下存储对ipfs哈希的引用。设置它的命令非常简单。

首先，您需要发布一些内容：
```sh
$ echo 'Let us have some mutable fun!' | ipfs add
```
请注意打印出来的哈希，并在此处使用它将其发布到网络：
```sh
$ ipfs name publish <that hash>
Published to <your peer ID>: <that hash>
```
现在，为了测试它是否有效，你可以尝试几种不同的东西：
```sh
$ ipfs name resolve <your peer ID>
<that hash>
```
如果你在同一台机器上运行它，它应该立即返回，因为你已经在本地缓存了该条目; 在运行ipfs的另一台计算机上试一试。

另一件要尝试的是在网关上查看它：
```sh
https://ipfs.io/ipns/<your peer ID>
```
所以，现在来到有趣的部分：让我们改变一切。
```sh
$ echo 'Look! Things have changed!' | ipfs add
```
接下来，从那里获取哈希并...
```sh
$ ipfs name publish <the new hash>
Published to <your peer ID>: <the new hash>
```
瞧！现在，如果再次解析该条目，您将看到新对象。

恭喜！您刚刚成功发布并解决了ipns条目！请注意，更新ipns条目可以“中断链接”，因为任何引用ipns条目的内容可能不再指向它预期的内容。没有办法解决这个问题（你知道，可变性），因此，如果你想确保持久性，就应该谨慎使用ipns链接。将来，我们可能会让ipns条目作为git提交链工作，每个连续的条目都指向其他值。

作者:[whyrusleeping](https://github.com/whyrusleeping)