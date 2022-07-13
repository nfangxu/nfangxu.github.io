## Http 请求转发

一行代码转发 Http 请求到指定服务, 支持通过配置修改请求及响应

### 使用
```bash
go get -d github.com/nfangxu/proxy
```

```go
package main

import (
	"github.com/nfangxu/proxy"
	"log"
	"net/http"
)

func main() {
	log.Fatal(http.ListenAndServe(":7788", http.HandlerFunc(proxy.NewSimpleProxy("https://foo.com").Proxy)))
}
```
