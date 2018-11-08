# 处理块
该ipfs add命令将从您指定的文件中的数据中创建Merkle DAG。执行此操作时，它遵循unixfs数据格式。这意味着您的文件被分解为块，然后使用“链接节点”以树状结构排列，将它们绑定在一起。给定文件的“散列”实际上是DAG中根（最上面）节点的散列。对于给定的DAG，您可以轻松查看其下的子块ipfs ls。

例如：
```sh
# ensure this file is larger than 256k
ipfs add alargefile 
ipfs ls thathash
```
上面的命令应该打印出如下内容：
```sh
ipfs@earth ~> ipfs ls qms2hjwx8qejwm4nmwu7ze6ndam2sfums3x6idwz5myzbn
qmv8ndh7ageh9b24zngaextmuhj7aiuw3scc8hkczvjkww 7866189 
qmuvjja4s4cgyqyppozttssquvgcv2n2v8mae3gnkrxmol 7866189 
qmrgjmlhlddhvxuieveuuwkeci4ygx8z7ujunikzpfzjuk 7866189 
qmrolalcquyo5vu5v8bvqmgjcpzow16wukq3s3vrll2tdk 7866189 
qmwk51jygpchgwr3srdnmhyerheqd22qw3vvyamb3emhuw 5244129
```
这将显示文件的所有直接子块，以及磁盘上它们及其子节点的大小。

## 块怎么办？
如果你喜欢冒险，你可以从这些不同的块中获得很多不同的信息。您可以使用子块哈希作为输入来ipfs cat仅查看任何给定子树中的数据（该块的数据及其子项）。要仅查看给定块的数据而不是其子项，请使用 。但要小心，因为在中间块上会将其DAG结构的原始二进制数据打印到屏幕上。ipfs block getipfs block get

ipfs block stat会告诉你给定块的确切大小（没有它的子节点），并ipfs refs会告诉你该块的所有子节点。同样，ipfs ls或将向您展示所有孩子及其大小。是一个更合适的命令，用于编写在给定对象的每个子块上运行的脚本。ipfs object linksipfs refs

## 块与对象
在IPFS中，块指的是由其密钥（散列）标识的单个数据单元。块可以是任何类型的数据，并且不一定具有与其相关联的任何格式。另一方面，对象是指遵循Merkle DAG protobuf数据格式的块。它可以通过命令进行解析和操作。任何给定的散列可以表示对象或块。ipfs object

## 从头开始创建块
创建自己的块很容易！只需将您的数据放在一个文件中并在其上运行即可 。或者，您可以管理您的filedata ，如下所示：ipfs block put <yourfile>ipfs block put
```sh
$ echo "This is some data" | ipfs block put
QmfQ5QAjvg4GtA3wg3adpnDJug8ktA1BxurVqBD8rtgVjM
$ ipfs block get QmfQ5QAjvg4GtA3wg3adpnDJug8ktA1BxurVqBD8rtgVjM
This is some data
```
注意：创建自己的块数据时，您将无法读取数据 ipfs cat。这是因为您输入的原始数据没有unixfs数据格式。要读取原始块，请使用示例中所示的内容。ipfs block get

作者:[whyrusleeping](https://github.com/whyrusleeping)