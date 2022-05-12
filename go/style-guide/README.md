# Go 编码规范

## 目录

- [Empty string check](#empty-string-check)
- [Error strings](#error-strings)
- [Handle errors](#handle-errors)
- [Imports](#imports)
- [Goroutine lifetimes](#goroutine-lifetimes)
- [In-band errors](#in-band-errors)
- [Use consistent spelling of certain words](#use-consistent-spelling-of-certain-words)


## Emtpy string check

Do this:

```go
if s == "" {
	...
}
```

Don't do this:

```go
if len(s) == 0 {
	...
}
```

参考: https://dmitri.shuralyov.com/idiomatic-go#empty-string-check.


## Error strings

Do this:

```go
fmt.Errorf("something bad")
```

Don't do this:

```go
fmt.Errorf("Something bad.")
```

说明: 错误字符串不要以大写字母开头,也不要包括标点符号。

参考: https://github.com/golang/go/wiki/CodeReviewComments#error-strings.


# Handle errors

Do this:

```go
resp, err := http.Get("http://example.com/")
if err != nil {
	// handle error
}

// ...
```

Don't do this:

```go
resp, _ := http.Get("http://example.com/")

// ...
```

说明:

- 不要用 `_` 来忽略错误
- 总是处理错误: return, 打日志, 或者极端情况下直接 panic

参考: https://github.com/golang/go/wiki/CodeReviewComments#handle-errors.


## Imports

Do this:

```go
import (
	"fmt"
	"os"

	"github/Meiqia/pkg"
)
```

Don't do this:

```go
import (
	"fmt"
	"os"
	"github/Meiqia/pkg"
)
```

说明:

- import 语句至少分为两组: 标准库, 其他库
- 也可以分为三组: 标准库, 三方库, 本地库

参考:

- https://github.com/golang/go/wiki/CodeReviewComments#imports
- https://github.com/uber-go/guide/blob/master/style.md#import-group-ordering


## Goroutine lifetimes

启动一个 goroutine 之前, 必须要知道它应该如何终止。

等待 goroutine 终止的常用方式:

- 自定义 channel
- [sync.WaitGroup](https://pkg.go.dev/sync#WaitGroup)

参考:

- https://github.com/golang/go/wiki/CodeReviewComments#goroutine-lifetimes
- [Never start a goroutine without knowning when it will stop](https://dave.cheney.net/practical-go/presentations/qcon-china.html#_never_start_a_goroutine_without_knowning_when_it_will_stop)


## In-band errors

Do this:

```go
// Case 1: 不会返回错误
func Get(key string) (value string, ok bool) {
	...
}

func main() {
	v, ok := Get("foo")
	if !ok {
		return // 处理无效的 v
	}

	// 处理有效的 v
}

// Case 2: 可能会返回错误
func Load(key string) (value string, err error) {
	...
}

func main() {
	v, err := Load("foo")
	if err == ErrNotFound {
		return // 处理无效的 v
	}
	if err != nil {
		return // 处理其他错误
	}

	// 处理有效的 v
}
```

Don't do this:

```go
// Case 1: 不会返回错误
func Get(key string) string {
	...
}

func main() {
	v := Get("foo")
	if v == "" {
		return // 处理无效的 v
	}

	// 处理有效的 v
}

// Case 2: 可能会返回错误
func Load(key string) (value string, err error) {
	...
}

func main() {
	v, err := Load("foo")
	if v == "" {
		return // 处理无效的 v
	}
	if err != nil {
		return // 处理其他错误
	}

	// 处理有效的 v
}
```

参考: https://github.com/golang/go/wiki/CodeReviewComments#in-band-errors.

golangci-lint linter: [nilnil](https://golangci-lint.run/usage/linters/#nilnil).


## Use consistent spelling of certain words

Do this:

```
// marshaling
// unmarshaling
// canceling
// canceled
// cancellation
```

Don't do this:

```
// marshalling
// unmarshalling
// cancelling
// cancelled
// cancelation
```

参考: https://dmitri.shuralyov.com/idiomatic-go#use-consistent-spelling-of-certain-words.