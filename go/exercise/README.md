# Go

## 问题 1

以下代码会输出什么?

```go
package main

import (
	"fmt"
)

func add(a []int) {
	a = append(a, 2)
}

func main() {
	a := []int{1}
	add(a)
	fmt.Println(a)
}
```

<details>
  <summary> 答案 </summary>

```
a: [1]
```

原理: 切片与底层数组的关系

参考: [The hidden risk of passing slice as function parameter](https://www.pixelstech.net/article/1607859246-The-hidden-risk-of-passing-slice-as-function-parameter)

</details>
