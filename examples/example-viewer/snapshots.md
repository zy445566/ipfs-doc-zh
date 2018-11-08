# 快照
让我们快速了解ipfs如何用于拍摄基本快照。

保存目录：
```sh
$ ipfs add -r ~/code/myproject
```
注意哈希：
```sh
$ echo $hash `date` >> snapshots
```
或者一下子：
```sh
$ echo `ipfs add -q -r ~/code/myproject | tail -n1` `date` >> snapshots
```
（`注意：使输出只包含哈希值，管道通过 确保只输出顶层文件夹的哈希值。`）-qtail -n1

确保拥有挂载点的占位符：
```sh
$ sudo mkdir /ipfs /ipns
$ sudo chown `whoami` /ipfs /ipns
```
您需要Fuse在计算机上安装才能mount从ipfs中获取目录。您可以在文档中找到有关如何安装`Fuse`的[说明](https://github.com/ipfs/go-ipfs/blob/master/docs/fuse.md)go-ipfs

实时查看快照：
```sh
$ ipfs mount
$ ls /ipfs/$hash/

# can also

$ cd /ipfs/$hash/
$ ls
```
通过`Fuse`接口，您将能够完全像拍摄快照时那样访问文件。

作者:[whyrusleeping](https://github.com/whyrusleeping)