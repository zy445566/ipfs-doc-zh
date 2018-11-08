# 固定文件
固定是ipfs中非常重要的概念。ipfs语义试图让每个单独的对象都是本地的，没有“从远程服务器为我检索这个文件”，只是ipfs cat或者无论实际对象位于何处都以相同的方式行事。虽然这很好，但有时你希望能够控制你所处的东西。固定是一种机制，允许您告诉ipfs始终将给定对象保持为本地。ipfs有一个相当积极的缓存机制，它会在你对它执行任何ipfs操作后将对象保持在本地很短的时间，但这些对象可能会定期进行垃圾收集。为了防止垃圾收集，只需固定你关心的哈希。默认情况下，通过递归方式固定添加的对象。ipfs getipfs add

```sh
echo "ipfs rocks" > foo
ipfs add foo
ipfs pin ls --type=all
ipfs pin rm <foo hash>
ipfs pin rm -r <foo hash>
ipfs pin ls --type=all
```
您可能已经注意到，第一个ipfs pin rm命令没有用，它应该警告您给定的哈希是“递归固定”的。ipfs世界中有三种类型的引脚; 直接引脚，只引脚单个引脚，而不涉及其他引脚。递归引脚，它固定一个给定的块及其所有子节点，以及间接引脚，它们是递归固定给定块父节点的结果。

固定的对象不能被垃圾收集，如果你不相信我试试这个：
```sh
ipfs add foo
ipfs repo gc
ipfs cat <foo hash>
```
但是，如果foo以某种方式取消固定......
```sh
ipfs pin rm -r <foo hash>
ipfs repo gc
ipfs cat <foo hash>
```
作者:[whyrusleeping](https://github.com/whyrusleeping)