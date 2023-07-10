# Go面向接口编程
> `interface` 可以将不同类型绑定到同一公共方法，实现`golang`多态，让开发程序变得更灵活
> `golang`的面向对象依赖于`interface`，万物皆可对象化、万物皆可接口化
---
## go的接口特点
- 高内聚低耦合，接口里所有方法都不能包含方法的具体实现
- 隐性实现，只要包含接口的所有方法即实现该接口，不需要特别声明实现某种接口
---

## go接口的细节
1. 本身不能创建实例
2. 可以多重继承
3. 不可以包含变量
4. 可以包含多种方法，但不能包含方法体
5. 无论多少重继承，不能有重复的方法名
6. 无需指定实现哪种接口，只需且必须包含接口的所有方法，即实现了该接口
7. 接口继承后，要实现改接口就必须实现改接口包括所有被继承的接口的所有方法
8. 所有自定义数据类型都可以实现接口，包括但不限于结构体
9. 一个类型可以同时实现多个接口
10. `interface`类型是引用，非值类型，默认是一个指针，若不初始化则返回`nil`
11. 空接口不包含任何方法，即所有类型都实现了空接口
---
## go接口的简单使用
- 声明  
接口的方法不可以包含方法体，但可以有传入值类型及返回值类型
```go
// ...
type InterfaceName interface {
	InterfaceMethod1(int,int) int
	InterfaceMethod2()
}
// ...
```
- 实现  
实现接口时要按照接口的规格全部实现其方法
```go
// ...
type MyType struct {
}
func (t MyType) InterfaceMethod1(a int, b int) int {
	return a + b
}
func (t MyType) InterfaceMethod2() {
	print("this method2")
}
// ...
```
- 使用
```go
package main
// ...
func main() {
	var t InterfaceName
	t = MyType{}
	print(t.InterfaceMethod1(1, 2))
	t.InterfaceMethod2()
}
```
