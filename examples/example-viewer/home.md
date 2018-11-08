# IPFS示例
这是ipfs示例的简单查看器。它的灵感来自@mbostock的华丽的网站bl.ocks.org。它呈现降价以便于阅读。您可以查看其工作原理，或在以下位置提供更改： https://github.com/protocol/ipfs-app-examples/tree/master/ipfs-examples

试试看！

## 编写自己的例子
编写自己的ipfs示例以与此工具一起使用是微不足道的。

* [步骤1.安装IPFS](https://ipfs.io/docs/examples/example-viewer/example#step-1-install-ipfs)
* [步骤2.捆绑示例](https://ipfs.io/docs/examples/example-viewer/example#step-2-bundle-the-example)
* [步骤3.发布！](https://ipfs.io/docs/examples/example-viewer/example#step-3-publish)

## 步骤1.安装IPFS

为了将示例发布到ipfs网关，您需要ipfs。了解如何在此处安装它：http://ipfs.io。要检查是否已安装，请输入：

```sh
> ipfs version
ipfs version 0.1.7
```
要在Web上查看它们，您需要运行守护程序：

```sh
> ipfs daemon
Initializing daemon...
API server listening on /ip4/127.0.0.1/tcp/5001
Gateway server listening on /ip4/127.0.0.1/tcp/8080
```
这将使您可以在浏览器中查看示例。而且，如果您连接到互联网，那么其他人可以通过ipfs网关查看它们。

## 步骤2.捆绑示例
该查看器遵循非常简单的格式。这只是一个目录！将所有文件放入目录中。此查看器将以不同方式处理几个特别命名的文件：

* `readme.md` 这是你的例子中的大部分，在降价时。
* `thumbnail.png` 这是缩略图。它也可以是thumbnail.jpg或thumbnail.gif。优先顺序是：png -> jpg -> gif
而已！努力将您需要的任何其他文件添加到目录中，而不是进行任何外部链接。这个想法是一个例子应该是完全独立的。

您可以随时按照发布中的相同步骤查看您的示例。

# 第3步。发布！
准备好发布（或只查看）您的示例后，请输入。为了我们：ipfs add -r <your directory>

```sh
> ipfs add -r my-example
added QmWWQSuPMS6aXCbZKpEjPHPUZN2NjB3YrhJTHsV4X3vb2t my-example/readme.md
added QmT4AeWE9Q9EaoyLJiqaZuYQ8mJeq4ZBncjjFH9dQ9uDVA my-example/thumbnail.jpg
added QmT9qk3CRYbFDWpDFYeAv8T8H1gnongwKhh5J68NLkLir6 my-example
```
现在，取你得到的最后一个哈希 - 为目录 - 并创建一个这样的URL：
```
http://localhost:8080/ipfs/<hash-of-the-viewer>/example#/ipfs/<hash-you-got>
```
例如：
```
http://localhost:8080/ipfs/QmPDgUhqWE4WqRqAHHtjUTkTjWRiSKGtAHtf8YWcqUiUvA/example#/ipfs/QmT9qk3CRYbFDWpDFYeAv8T8H1gnongwKhh5J68NLkLir6
```
如果已连接，您还可以在公共ipfs网关上查看它：
```
http://ipfs.io/ipfs/QmPDgUhqWE4WqRqAHHtjUTkTjWRiSKGtAHtf8YWcqUiUvA/example#/ipfs/QmT9qk3CRYbFDWpDFYeAv8T8H1gnongwKhh5J68NLkLir6
```
额外奖励：使用makefile发布
我喜欢用简单的方式发布我的例子Makefile：
```sh
# Makefile that publishes this example

viewer = "QmPDgUhqWE4WqRqAHHtjUTkTjWRiSKGtAHtf8YWcqUiUvA"
local = "http://localhost:8080/ipfs/"
gway = "http://ipfs.io/ipfs/"

publish: $(shell find . )
  @hash=$(shell ipfs add -r -q . | tail -n1); \
    echo $(local)/$(viewer)/example#/ipfs/$$hash; \
    echo $(gway)/$(viewer)/example#/ipfs/$$hash

# we need ; and escaped newlines to capture the variable
```
现在你可以：
```sh
> make publish
http://localhost:8080/ipfs/QmPDgUhqWE4WqRqAHHtjUTkTjWRiSKGtAHtf8YWcqUiUvA/example#/ipfs/QmT9qk3CRYbFDWpDFYeAv8T8H1gnongwKhh5J68NLkLir6
http://ipfs.io/ipfs/QmPDgUhqWE4WqRqAHHtjUTkTjWRiSKGtAHtf8YWcqUiUvA/example#/ipfs/QmT9qk3CRYbFDWpDFYeAv8T8H1gnongwKhh5J68NLkLir6
:)
```

重要的提示:

`请注意，您的示例本身不会停留在ipfs Web上。ipfs网关暂时缓存一些东西，但不是永远。如果你想永久地坚持下去，请在这里阅读（todo：添加链接）。`