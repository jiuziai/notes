# Go语言接口
> interface 可以将不同类型绑定到同一公共方法，实现golang多态，让开发程序变得更灵活

## 例子
```go
package main

import "fmt"

type Humans interface {
	call()
}

type Man struct {
	name string
}
type Woman struct {
	name string
}

func (man Man) call() {
	fmt.Printf("我是英俊潇洒的%s\n", man.name)
}

func (woman Woman) call() {
	fmt.Printf("我是貌美如花的%s\n", woman.name)
}

func main() {
	var human Humans
	human = Man{name: "老王"}
	human.call()
	human = Woman{name: "李万姬"}
	human.call()
}

```