# 使用graphmd绘制可视化对象
![graph_obj.png](./graph_obj.png)
当使用ipfs存储文件或编写更复杂的数据结构时，可视化正在创建的merkledag通常非常有用。为此，我写了一个名为graphmd（graph merkle dag）的简单工具。

graphmd是一个非常短的shell脚本（[源代码](https://ipfs.io/docs/examples/example-viewer/graphmd)）。它使用 标志来产生输出。ipfs refs --formatdot

## 安装graphmd
graphmd 很快就会有自己的回购，但是现在你可以安装它：
```sh
ipfs cat Qmcd7Sebd46vxDWjbUERK8w82zp8sgWTtHT5c93kzr2v3M  >/usr/local/bin/graphmd
chmod +x /usr/local/bin/graphmd
```
## graphmd用法
```sg
> graphmd
usage: graphmd <ipfs-path>...
output merkledag links in graphviz dot

use it with dot:
  bin/graphmd QmZPAMWUfLD95GsdorXt9hH7aVrarb2SuLDMVVe6gABYmx | dot -Tsvg
  bin/graphmd QmZPAMWUfLD95GsdorXt9hH7aVrarb2SuLDMVVe6gABYmx | dot -Tpng
  bin/graphmd QmZPAMWUfLD95GsdorXt9hH7aVrarb2SuLDMVVe6gABYmx | dot -Tpdf
```
## 例子
### 鉴于此[演示](https://ipfs.io/ipfs/QmRCJXG7HSmprrYwDrK1GctXHgbV7EYpVcJPQPwevoQuqF)目录：
```sh
> tree demo
demo
├── cat.jpg
└── test
    ├── bar
    ├── baz
    │   ├── b
    │   └── f
    └── foo

2 directories, 5 files
```
### 将文件添加到ipfs
```sh
> ipfs add -r demo
added QmajFHHivh25Qb2cNbnnnEeUe1gDLHX9ta7hs2XKX1vazb demo/cat.jpg
added QmTz3oc4gdpRMKP2sdGUPZTAGRngqjsi99BPoztyP53JMM demo/test/bar
added QmTz3oc4gdpRMKP2sdGUPZTAGRngqjsi99BPoztyP53JMM demo/test/baz/b
added QmYNmQKp6SuaVrpgWRsPTgCQCnpxUYGq76YEKBXuj2N4H6 demo/test/baz/f
added QmX1ebVUtfY11ZCpVmqyE5mDoN62SpLd8eLPpg5GGV1ABt demo/test/baz
added QmYNmQKp6SuaVrpgWRsPTgCQCnpxUYGq76YEKBXuj2N4H6 demo/test/foo
added QmNtpA5TBNqHrKf3cLQ1AiUKXiE4JmUodbG5gXrajg8wdv demo/test
added QmRCJXG7HSmprrYwDrK1GctXHgbV7EYpVcJPQPwevoQuqF demo
```
### 使用graphmd生成的输出点
```sh
> graphmd QmRCJXG7HSmprrYwDrK1GctXHgbV7EYpVcJPQPwevoQuqF >graph.dot
digraph {
  graph [rankdir=LR];
  QmRCJXG7HSmprrYwDrK1GctXHgbV7EYpVcJPQPwevoQuqF [fontsize=8 shape=box];
  QmajFHHivh25Qb2cNbnnnEeUe1gDLHX9ta7hs2XKX1vazb [fontsize=8 shape=box];
  QmRCJXG7HSmprrYwDrK1GctXHgbV7EYpVcJPQPwevoQuqF -> QmajFHHivh25Qb2cNbnnnEeUe1gDLHX9ta7hs2XKX1vazb [label="cat.jpg"];
  QmRCJXG7HSmprrYwDrK1GctXHgbV7EYpVcJPQPwevoQuqF [fontsize=8 shape=box];
  QmNtpA5TBNqHrKf3cLQ1AiUKXiE4JmUodbG5gXrajg8wdv [fontsize=8 shape=box];
  QmRCJXG7HSmprrYwDrK1GctXHgbV7EYpVcJPQPwevoQuqF -> QmNtpA5TBNqHrKf3cLQ1AiUKXiE4JmUodbG5gXrajg8wdv [label="test"];
  QmNtpA5TBNqHrKf3cLQ1AiUKXiE4JmUodbG5gXrajg8wdv [fontsize=8 shape=box];
  QmTz3oc4gdpRMKP2sdGUPZTAGRngqjsi99BPoztyP53JMM [fontsize=8 shape=box];
  QmNtpA5TBNqHrKf3cLQ1AiUKXiE4JmUodbG5gXrajg8wdv -> QmTz3oc4gdpRMKP2sdGUPZTAGRngqjsi99BPoztyP53JMM [label="bar"];
  QmNtpA5TBNqHrKf3cLQ1AiUKXiE4JmUodbG5gXrajg8wdv [fontsize=8 shape=box];
  QmX1ebVUtfY11ZCpVmqyE5mDoN62SpLd8eLPpg5GGV1ABt [fontsize=8 shape=box];
  QmNtpA5TBNqHrKf3cLQ1AiUKXiE4JmUodbG5gXrajg8wdv -> QmX1ebVUtfY11ZCpVmqyE5mDoN62SpLd8eLPpg5GGV1ABt [label="baz"];
  QmX1ebVUtfY11ZCpVmqyE5mDoN62SpLd8eLPpg5GGV1ABt [fontsize=8 shape=box];
  QmTz3oc4gdpRMKP2sdGUPZTAGRngqjsi99BPoztyP53JMM [fontsize=8 shape=box];
  QmX1ebVUtfY11ZCpVmqyE5mDoN62SpLd8eLPpg5GGV1ABt -> QmTz3oc4gdpRMKP2sdGUPZTAGRngqjsi99BPoztyP53JMM [label="b"];
  QmX1ebVUtfY11ZCpVmqyE5mDoN62SpLd8eLPpg5GGV1ABt [fontsize=8 shape=box];
  QmYNmQKp6SuaVrpgWRsPTgCQCnpxUYGq76YEKBXuj2N4H6 [fontsize=8 shape=box];
  QmX1ebVUtfY11ZCpVmqyE5mDoN62SpLd8eLPpg5GGV1ABt -> QmYNmQKp6SuaVrpgWRsPTgCQCnpxUYGq76YEKBXuj2N4H6 [label="f"];
  QmNtpA5TBNqHrKf3cLQ1AiUKXiE4JmUodbG5gXrajg8wdv [fontsize=8 shape=box];
  QmYNmQKp6SuaVrpgWRsPTgCQCnpxUYGq76YEKBXuj2N4H6 [fontsize=8 shape=box];
  QmNtpA5TBNqHrKf3cLQ1AiUKXiE4JmUodbG5gXrajg8wdv -> QmYNmQKp6SuaVrpgWRsPTgCQCnpxUYGq76YEKBXuj2N4H6 [label="foo"];
}
```
### 用它来dot制作svg，pdf，png或任何
```sh
graphmd QmRCJXG7HSmprrYwDrK1GctXHgbV7EYpVcJPQPwevoQuqF | dot -Tsvg >output/graph.svg
graphmd QmRCJXG7HSmprrYwDrK1GctXHgbV7EYpVcJPQPwevoQuqF | dot -Tpdf >output/graph.pdf
graphmd QmRCJXG7HSmprrYwDrK1GctXHgbV7EYpVcJPQPwevoQuqF | dot -Tpng >output/graph.png
这里是：[svg](https://ipfs.io/ipfs/QmbefthRKDReojALJi8nGPwvUVPqe1aXdoD9ysX44aUfvG/graph.svg)，[pdf](https://ipfs.io/ipfs/QmbefthRKDReojALJi8nGPwvUVPqe1aXdoD9ysX44aUfvG/graph.pdf)，[png](https://ipfs.io/ipfs/QmbefthRKDReojALJi8nGPwvUVPqe1aXdoD9ysX44aUfvG/graph.png)
```
![graph_pics.png](./graph_pics.png)


文件块
graphmd对于检查文件阻止算法特别有用。例如，以下是ipfs使用默认的半平衡间接块分块的二进制文件：

* [点输出](https://ipfs.io/ipfs/QmQ8yWC1SGn73P1SPSw8iqSBGEscve1N6sQpzd1xzD5EV1/graph.dot)
* [svg渲染](https://ipfs.io/ipfs/QmQ8yWC1SGn73P1SPSw8iqSBGEscve1N6sQpzd1xzD5EV1/graph.svg)
* [pdf渲染](https://ipfs.io/ipfs/QmQ8yWC1SGn73P1SPSw8iqSBGEscve1N6sQpzd1xzD5EV1/graph.pdf)

![graph_file.svg](./graph_file.svg)

作者：[Juan Benet](https://github.com/jbenet)