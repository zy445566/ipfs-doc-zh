# 制作自己的ipfs服务
ipfs默认运行一些默认服务，例如dht，bitswap和诊断服务。这些中的每一个都只是在ipfs PeerHost上注册一个处理程序，并在其上侦听新的连接。该 corenet软件包具有非常干净的界面来实现此功能。因此，让我们尝试构建一个简单的演示服务来试试这个！

让我们从构建服务主机开始：
```go
package main

import (
    "fmt"

    core "github.com/ipfs/go-ipfs/core"
    corenet "github.com/ipfs/go-ipfs/core/corenet"
    fsrepo "github.com/ipfs/go-ipfs/repo/fsrepo"

    "code.google.com/p/go.net/context"
)
```
我们不需要太多的进口。现在，我们唯一需要的是我们的主要功能：

设置ipfsnode。
```go
func main() {
    // Basic ipfsnode setup
    r, err := fsrepo.Open("~/.ipfs")
    if err != nil {
        panic(err)
    }

    ctx, cancel := context.WithCancel(context.Background())
    defer cancel()

    cfg := &core.BuildCfg{
        Repo:   r,
        Online: true,
    }

    nd, err := core.NewNode(ctx, cfg)

    if err != nil {
        panic(err)
    }
```
这只是代码的基本模板，用于从users 目录中的config启动默认ipfsnode 。~/.ipfs

接下来，我们将构建我们的服务。
```go
    list, err := corenet.Listen(nd, "/app/whyrusleeping")
    if err != nil {
        panic(err)
    }

    fmt.Printf("I am peer: %s\n", nd.Identity.Pretty())

    for {
        con, err := list.Accept()
        if err != nil {
            fmt.Println(err)
            return
        }

        defer con.Close()

        fmt.Fprintln(con, "Hello! This is whyrusleepings awesome ipfs service")
        fmt.Printf("Connection from: %s\n", con.Conn().RemotePeer())
    }
}
```
这就是你在ipfs上编写服务所需的全部功能。当客户端连接时，我们向他们发送问候语，将他们的对等ID打印到我们的日志中，然后关闭会话。这是最简单的服务，您可以真正编写任何想要处理连接的内容。

现在我们需要一个客户端连接到我们：
```go
package main

import (
    "fmt"
    "io"
    "os"

    core "github.com/ipfs/go-ipfs/core"
    corenet "github.com/ipfs/go-ipfs/core/corenet"
    peer "github.com/ipfs/go-ipfs/p2p/peer"
    fsrepo "github.com/ipfs/go-ipfs/repo/fsrepo"

    "golang.org/x/net/context"
)

func main() {
    if len(os.Args) < 2 {
        fmt.Println("Please give a peer ID as an argument")
        return
    }
    target, err := peer.IDB58Decode(os.Args[1])
    if err != nil {
        panic(err)
    }

    // Basic ipfsnode setup
    r, err := fsrepo.Open("~/.ipfs")
    if err != nil {
        panic(err)
    }

    ctx, cancel := context.WithCancel(context.Background())
    defer cancel()

    cfg := &core.BuildCfg{
        Repo:   r,
        Online: true,
    }

    nd, err := core.NewNode(ctx, cfg)

    if err != nil {
        panic(err)
    }

    fmt.Printf("I am peer %s dialing %s\n", nd.Identity, target)

    con, err := corenet.Dial(nd, target, "/app/whyrusleeping")
    if err != nil {
        fmt.Println(err)
        return
    }

    io.Copy(os.Stdout, con)
}
```
这个客户端将设置他们的ipfs节点（注意：这是相当昂贵的，你通常不会只为一个连接启动一个实例）并拨打我们刚刚创建的服务。

要试用它，请在一台计算机上运行以下命令：
```sh
$ ipfs init # if you havent already
$ go run host.go
```
这应打印出对等ID，复制并在第二台机器上使用它：
```sh
$ ipfs init # if you havent already
$ go run client.go <peerID>
```
它应该打印出来 Hello! This is whyrusleepings awesome ipfs service

现在，你可能会问自己：“我为什么要使用它？它比net包装好吗？”。嗯，这里有优点：

1. 您拨打特定的peerID，无论他们当前的IP地址是什么。
2. 您可以利用我们的网络包中内置的NAT遍历。
3. 您可以获得更有意义的协议ID字符串，而不是“端口”号。

作者:[whyrusleeping](https://github.com/whyrusleeping)