## Golang 锁的接口定义

内置了内存锁, 支持自定义其他实现(Redis 等).

- 支持自定义实现
- 支持自动释放和手动释放
- 支持自定义设置过期时间

### 接口定义如下

```go
package main

import (
	"context"
	"time"
)

type ILocker interface {
	Lock(ctx context.Context, id string, ttl time.Duration) (ILocked, error)
	Locking(ctx context.Context, id string) bool
	UnLock(ctx context.Context, id string) error
}

type ILocked interface {
	Locking(ctx context.Context) bool
	UnLock(ctx context.Context) error
}
```

### 使用

```golang
package main

import (
	"context"
	"github.com/nfangxu/ilocker"
	"time"
)

func main() {
    var locker ilocker.ILocker

	locker = ilocker.NewMemoryLocker(time.Minute) // 参数为轮询自动释放的间隔时间, 若传入 0 则使用默认值(time.Minute)
	ld, err := locker.Lock(context.TODO(), "foo", time.Second)
	if err != nil {
		panic(err)
	}
	defer ld.UnLock(context.TODO())
	// TODO: do something
}

```
