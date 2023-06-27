# Golang的"面向对象"
> ### Golang是一种面向过程, 简洁, 静态的类C语言, 所以并没有类的概念
> ### 故Golang使用面向过程思想(实现面向对象的三大特性)的方式同C相同
***
- ## 封装
    在Golang包中, 小写开头即为私有,大写开头即为公有
```go
package test

// 公有结构体(结构体可看作类,结构体成员可看作类的属性),结构体不公有则包外无法访问或者创建切片
type User struct {
    name string
    age int
    height int
}
// 私有方法(可看作类的方法),包内可访问,包外不可访问
func (u user) sortBy() int{
    return u.age
}
// 公有方法(可看作类的方法),包内包外都可访问
func (u user) Sort(leftUser user, right user){

}
```