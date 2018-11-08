# 添加和播放视频
ipfs可用于存储和播放视频。假设我们添加了一个视频：
```sh
ipfs add -q sintel.mp4 | tail -n1
```
获取生成的哈希，您可以通过几种不同的方式查看它：

在命令行上：
```sh
ipfs cat $vidhash | mplayer -vo xv -
```
通过本地网关：
```sh
mplayer http://localhost:8080/ipfs/$vidhash

# or open it up in a tab in chrome (or firefox)

chromium http://localhost:8080/ipfs/$vidhash
```
（`注意：网关方法适用于大多数视频播放器和浏览器`）

作者:[whyrusleeping](https://github.com/whyrusleeping)