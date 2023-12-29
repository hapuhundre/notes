# Go Tour

## Basic

### 包，变量与函数

```go
package main

import "fmt"
// 形参简化
func minus(x, y int) int {
    return x-y
}
// 多值返回
func swap(x, y string) (string, string) {
	return y, x
}
// 函数声明中命名返回值 
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}
// 变量的声明
var c, python, java bool
func printVar(x int) {
    fmt.Println(x, c, python, java)
}
// 变量的初始化
var i, j int = 1, 2
// := 短变量的声明
func printVar2() {
    var i, j int = 1, 2
	k := 3
	c, python, java := true, false, "no!"
    fmt.Println(c, python, java, k, i, j)
}
/* go的基本变量类型
bool

string

int  int8  int16  int32  int64

uint uint8 uint16 uint32 uint64 uintptr

byte // uint8 的别名

rune // int32 的别名 // 表示一个 Unicode 码点

float32 float64

complex64 complex128

没有明确初始值的变量声明会被赋予它们的 零值: 0, false, ""
*/
// 类型转换，显式地调用函数
func typecast() {
    var i int = 42
    var f float64 = float64(i)
}
// 类型推导
func typeg() {
	v := 42.0 // 修改这里！
	fmt.Printf("v is of type %T\n", v) // float64
}
// const, 不能用:=
func declareConst() {
  const Pi = 3.14
}
// 数值常量，带有类型推导
const (
	Big = 1 << 100
	Small = Big >> 99
)
```

### 控制语句：for, if, else, switch和defer

```go
// for loop
sum := 1
for i := 0; i < 10; i++ {
	sum += i
}
// 可缺省的for
for ; sum < 1000; {
	sum += sum
}
// “while”
for sum<1000 {
    sum += sum;
}
// endless loop
for {
    // do something
}
// if
func is_odd(x int) bool{
    if x % 2 == 0 {
        return false
    }
    return true
}
// if前面执行一个简单语句
func pow_is_odd(x int) bool {
    if v := math.Pow(x,2); v % 2 == 0 {
        return false
    }
    return true
}
// else 略
// exercise: 牛顿法求平方根
func Sqrt(x float64) float64 {
    const threshold = 0.00001
	z := 1.0
	for  math.Abs(z*z - x) > threshold {
	    z -=  (z*z - x) / (2*z)
	}
	return z
}
// switch, 没有break
switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		fmt.Printf("%s.\n", os)
}
// switch 的 case 语句从上到下顺次执行，直到匹配成功时停止。
// 没有条件的switch, 代替if-elif-else
switch {
    case x > 123:
        fmt.Println("gaga")
    case x < 124:
        fmt.Println("gagaga")
}
// defer, 退出函数时才运行
func fuxx() {
    defer fmt.Println("you!")
    fmt.Println("fuck ")
}
// defer is a stack
func reverse_print() {
    for i:=0; i<10; i++{
        defer fmt.Println(i)
    }
}
```



###  struct, slice和映射

```go
// go指针的零值为nil, 没有指针运算(++)
var p *int
i := 42
p = &i
// 结构体
type Node struct {
    x int
    y int
}
var n = Node{1,3}
// 字段
fmt.Println(n.x)
// 结构体指针，引用字段时，注意直接用.,而不是->, 和c++的引用类似
p = &n
fmt.Println(p.x)
// 初始化
Node{} // x y 0
Node{x:1} // x 1, y 0
// 数组
primes := [6]int{2, 3, 5, 7, 11, 13}
gaga := []string{"gaga", "ga"}
// 切片 [low, high)
small_primes := primes[1:4] // 1到3的数
var small_primes []int = primes[1:4] // 和上面的形式等价
// 切片是引用
primes[1] = 17 // small_primes第一个值也会随之变动
// 嵌套的切片文法
	s := []struct {
		i int
		b bool
	}{
		{2, true},
		{3, false},
		{5, true},
		{7, true},
		{11, false},
		{13, true},
    }
// 切端点
s := []int{1,2,3,4,5,6}
s = s[:] // 全切
s = s[:2]
s = s[3:]
// 切片的对象本质上还是在原来的基础上滑动
s = s[:0] //清空 s=[]
s = s[:4] // s= [1,2,3,4]
// 切片的零值为nil
s = []int // a nil var
// make一个指定长度的切片
new_s := make([]string, 5)
// 切片有长度(len)和容量两个属性
s := []int{1,2,3,4,5,6}
s := s[:0] // 截取操作，len=0, cap=6
s := s[:6] // 扩展操作 len=6,cap=6
s := s[6:] // 舍弃操作，len=0, cap=0
s := s[:1] // 会报错
// 多维切片
twod_str := [][]string{
    {"gaga"}, {"gagaga"}}
twod_str[0][0] =  "gagaga"
// 向切片增加元素
var s []int
s = append(s, 1,3,4,5)
// range
var nums = []int{1,21,23,43,5,6}
for i, v := range nums {
    // i index, v value
}
// range with placeholder
for i, _ := range nums{
    // or for _, v := range nums
}
// exercise: 切片
package main
import "golang.org/x/tour/pic"
    
func Pic(dx, dy int) [][]uint8 {
    pic := [][]uint8{}
	for y := 0; y<dy; y++ {
	    line := []uint8{}
	    for x := 0; x<dx; x++ {
		    line = append(line, uint8(x*y))
		}
		pic = append(pic, line)
	}
	return pic	    
}

func main() {
	pic.Show(Pic)
}
// have a relax~
```



exercise output:

![](slice_output.png)



```go
// 映射 map, 零值为nil
m = make(map[string]int)
m["gaga"] = 3
// 初始化方法
m = map[string]string{"gaga":"yoxi", "baga":"memeda"}
// map的find
ele, is_found := m["toxi"] // ele 为nil，is_found为false
// exercise 字符统计
func WordCount(s string) map[string]int {
	words := strings.Fields(s)
	words_map := map[string]int{}
	for _, word := range words {
		_, existed := words_map[word]
		if existed {
		    words_map[word] += 1
		} else {
			words_map[word] = 1
		}
	}
	return words_map
}
// 函数也可以作为形参
func add(a int, b int) int {
    return a+b
}
func compute(fn func(int, int) int) int {
    return fn(3,5)
}
// 闭包, 下面这个例子很有functional的味道
func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}

func main() {
	pos, neg := adder(), adder()
	for i := 0; i < 10; i++ {
		fmt.Println(
			pos(i), // 1+2+3+4, 
			neg(-2*i),
		)
	}
}
// exercise Fibonacci
func fibonacci() func() int {
	f_n2, f_n1 := 0, 1
	return func() int {
		f_n := f_n2
		f_n1, f_n2 = f_n1+f_n2, f_n1
		return f_n
	}
}
```



## Method & API

```go
// go没有class，提供结构体的方法
type Node struct {
    x, y float64
}

func (n Node) dist(other Node) float64{
    return math.Sqrt(math.Pow(n.x-other.x, 2) + math.Pow(n.y-other.y, 2))
}
// 方法就是函数，不能为内建的类型定义方法
type MyString string

func (str MyString) size() int {
    return len(str)
}
// 指针即引用
func (n *Node) Move(dis float64){
    n.x += dis
    n.y += dis
}
Move(&node) // 指针类型得指定
// go没有->,都是 .
p := &node
n_x = p.x

// 接口的集合,类似于抽象类
type Server interface {
    Run() 
    SetOptions() int
    Init() int
}
type T struct {
    str string
}
// 实现了Server里面的Run，但不用显式的声明
func (msg T) Run() {
    fmt.Println(msg)
}
var server Server = T{"gaga"} // 通过这样用server把定义和接口进行分离
server.Run()

// 接口值
type Str string
func (s Str) Run() {
    fmt.Println(s)
}
server = Str{"yoxi"} // 接口可以转换赋值，其绑定的类型由T变为Str

// 可以检验下接口内的具体指针是否为空
func (msg *Str) Run() {
    if msg == nil{
        fmt.Println("err")
        return 
    }
    fmt.Println(msg)
}
// 空接口
var i interface() // (<nil>, <nil>)
i = 42 // (42, int)
i = "gaga" // (gaga, string)

// 类型断言
s, is_string = server.(Str) // is_string 是个bool量

// 类型选择
switch v := server.(type) {
    case Str:
    // TODO
    case F:
    // TODO
    default:
}

// exercise: byte[4] 转字符串 fmt.Stringer
// 给 IPAddr 添加一个 "String() string" 方法
func (ip IPAddr) String() string{
	ip_str := ""
	i := 0
	for ; i<len(ip)-1 ;i++ {
		ip_str += (strconv.Itoa(int(ip[i]))+".")
	}
	ip_str += strconv.Itoa(int(ip[i]))
	return ip_str
}

// 内建的ERROR类型，例如连接mysql数据库
type MySQLError struct { // example, 一般db connector会定义好error类型
    ErrorCode int
    ErrorMsg string
}

db_conn, err := sql.Open("mysql", "user:password@/db_name")
if err != nil {
    fmt.Println(err)
}

// exercise ERROR
type ErrNegativeSqrt float64

func (e ErrNegativeSqrt) Error() string{
	return fmt.Sprint("cannot Sqrt negative number: ",float64(e))
}

func Sqrt(x float64) (float64,error) {
	if x < 0 {
		return x, ErrNegativeSqrt(x)
	}
	const threshold = 0.00001
	z := 1.0
	for  math.Abs(z*z - x) > threshold {
	    z -=  (z*z - x) / (2*z)
	}
	return z, nil
}

// 读取数据流，例如读取一个文件，然后buffer的单位为30
func main() {
    content, err := ioutil.ReadFile("reader.go")
	if err != nil {
        log.Fatal(err)
	}

	r := strings.NewReader(string(content))
	b := make([]byte, 30)
	for {
		n, err := r.Read(b)
		fmt.Printf("n = %v err = %v b = %v\n", n, err, b)
		fmt.Printf("b[:n] = %q\n", b[:n])
		if err == io.EOF {
			break
		}
	}
}

// exercise 无限的‘A'流
type MyReader struct{}

func (reader MyReader) Read(bytes []byte) (int, error) {
    for i := range bytes {
		bytes[i] = 'A'
	}
	return len(bytes), nil
}

func main() {
	r := MyReader{}
	
	b := make([]byte, 8)
	for {
		n, err := r.Read(b)
		fmt.Printf("n = %v err = %v b = %v\n", n, err, b)
		fmt.Printf("b[:n] = %q\n", b[:n])
		if err == io.EOF {
			break
		}
	}
}

// exercise rot13 密码
func rot13_byte(b byte) byte{ 
	s := rune(b)
	if s >= 'A' && s <= 'M' || s >= 'a' && s <= 'm' {
		b += 13
	}
	if s >= 'N' && s <= 'Z' || s >= 'n' && s <= 'z' {
		b -= 13
	}
	return b
}

type rot13Reader struct {
	r io.Reader
}

func (rot13 rot13Reader) Read(bytes []byte) (int, error) {
	res, err := rot13.r.Read(bytes)
	if err != nil {
		return -1, err
	}
	for i:=0; i<res; i++ {
	    bytes[i] = rot13_byte(bytes[i])
	}
	return len(bytes), nil
}

func main() {
	s := strings.NewReader("Lbh penpxrq gur pbqr!")
	r := rot13Reader{s}
	io.Copy(os.Stdout, &r)
}

// image
import (
	"golang.org/x/tour/pic"
	"image"
	"image/color"
)

type Image struct{
	w, h int
}

func (img Image) Bounds() image.Rectangle {
	return image.Rect(0,0,img.w, img.h)
}

func (img Image) ColorModel() color.Model {
    return color.RGBAModel
}

func (img Image) At(w int, h int) color.Color {
	return color.RGBA{uint8(w),uint8(h),123,123}
}

func main() {
	m := Image{123, 123}
	pic.ShowImage(m)
}
```



## Concurrency

```go
// goroutine, 类似于std::thread
func say(msg string) {
  // do something
}
go say("world")
say("hello")

// 信道 带类型的管道
func sum(s []int, c chan int) {
	sum := 0
	for _, v := range s {
		sum += v
	}
	c <- sum // 将和送入 c
}

func main() {
	s := []int{7, 2, 8, -9, 4, 0}

	c := make(chan int)
	go sum(s[:len(s)/2], c)
	go sum(s[len(s)/2:], c)
	x, y := <-c, <-c // 从 c 中接收

	fmt.Println(x, y, x+y)
}

// 信道带缓冲, 溢出会造成deadlock错误
c := make(chan int, 100)

// 使用range+close观察goroutine中的状态 
// 例子：机器人跑轨迹，主线程观察并输出结果。
func running(traj []int, c chan int) {
	for i:=0;i<len(traj);i++ {
        c <- traj[i]
        time.Sleep(1*time.Second)
	}
	close(c) // 使用range必须关闭信道
}

s := []int{1, 2, 8, -9, 4, 0}
c := make(chan int)
go running(s, c)
for i := range c {
	fmt.Println("running at ", i)
}

// select语句，多通信操作
// 例子：机器人跑轨迹，能接收pause信道
func running(traj []int, c, pause chan int) {
	for i:=0;i<len(traj);i++ {
		select {
			case c <- traj[i]:
                time.Sleep(1*time.Second)
			case code := <- pause:
				fmt.Println("received pause, ", code)
				close(c)
				return
		}				
	}
	close(c)
}

s := []int{1, 2, 8, -9, 4, 0}

c := make(chan int)
pause := make(chan int)
go running(s, c, pause)
for i := range c {
	fmt.Println("running at ", i)
	if i < 0 {
		pause <- -1
	}
}

// select 最好加个default，表示信道在阻塞或未发送的情况

// exercise binary tree
// Walk 步进 tree t 将所有的值从 tree 发送到 channel ch。
func WalkRecursive(t *tree.Tree, ch chan int) {
    if t != nil {
        WalkRecursive(t.Left, ch)
        ch <- t.Value
        WalkRecursive(t.Right,ch)
    }
}

func Walk(t *tree.Tree, ch chan int) {
    WalkRecursive(t, ch)
    close(ch)
}

// Same 检测树 t1 和 t2 是否含有相同的值。
func Same(t1, t2 *tree.Tree) bool {
	ch1, ch2 := make(chan int,100), make(chan int,100) // 如果不设缓冲值，可能发生deadlock错误
	Walk(t1, ch1)
	Walk(t2, ch2)
	for {
		v1, ok1 := <-ch1
		v2, ok2 := <-ch2
		if ok1 != ok2 || v1 != v2{
		  return false // should close channel?
		}
		if !ok1 || !ok2 {
		  break
		}
	}
	return true
}

```

