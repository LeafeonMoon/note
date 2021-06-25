# Go

[TOC]

# 0 相關地址

**[下載I](https://golang.org/dl/)**

**[下載II](https://golang.google.cn/dl/)**

**[菜鳥 Golang](https://www.runoob.com/go/go-tutorial.html)**

**[C語言中文網 Golang](http://c.biancheng.net/golang/)**



# 1 安裝

## 1.1 Arch

```bash
sudo pacman -S go
```



# 2 GO 環境

## 2.1 控制台輸出 GO 環境

```bash
go env
```

## 2.2 設置 GO111MODULE

值：off、on、auto（默認）

```bash
# go 1.3
go env -w GO111MODULE=on

# 初始化 mod
# 在 GO 項目的根目錄下
go mod init 「mod 名稱」
```

## 2.3 設置 GOPROXY

| 提供方 |                                     |
| ------ | ----------------------------------- |
| 默認   | https://proxy.golang.org            |
| 阿里雲 | https://mirrors.aliyun.com/goproxy/ |
| 七牛雲 | https://goproxy.cn                  |

```bash
# 設置 GOPROXY，可解決大陸無法 go get 下載依賴問題（大陸推薦設置為七牛雲）。
# 「,direct」加載鏈接後
go env -w GOPROXY=https://goproxy.cn,direct
```

## 2.4 編譯

可設置 GOOS、GOARCH 兩個值來編譯 。

- GOOS - 編譯系統

- GOARCH，編譯框架

| OS      | ARCH              | OS Version                 |
| ------- | ----------------- | -------------------------- |
| Linux   | 386 / amd64 / arm | >= Linux 2.6               |
| darwin  | 386 / amd64       | OS X (Snow Leopard + Lion) |
| freebsd | 386 / amd64       | >= FreeBSD 7               |
| windows | 386 / amd64       | >= Windows 2000            |

### 2.4.1 Example

1. 編譯 amd64 框架的 Windows，設置 GOOS 和 GOARCH，設置完畢可統計 `go vnv` 查看這兩個值的情況。


```bash
go set GOOS=windows
go set GOARCH=amd64
```

2. 編譯，需要在擁有 main 方法的文件所在目錄下執行以下命令。

```bash
# xxx 為擁有 main 方法的文件
go build xxx.go
```



# 3 依賴包

## 3.1 Web 框架

### 3.1.1 gin

[官網](https://gin-gonic.com/)

[Github](https://github.com/gin-gonic/gin)

[簡體中文文檔](https://learnku.com/docs/gin-gonic/2019)

```bash
go get -u github.com/gin-gonic/gin 
```

## 3.2 日誌

### 3.2.1 logrus

[Gihhub](https://github.com/Sirupsen/logrus)

```bash
go get -u github.com/gin-gonic/gin
```

### 3.2.2 zap

[GitHub](https://github.com/uber-go/zap)

```bash
go get -u github.com/uber-go/zap
```

## 3.4 文件分割

[Github](https://github.com/lestrrat-go/file-rotatelogs)

可用於日誌文件分割。

```bash
go get -u github.com/lestrrat-go/file-rotatelogs
```

## 3.5 Redis

```bash
go get -u github.com/go-redis/redis
```
