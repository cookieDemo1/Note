###### 1.go中的iniit函数

- 在导入包的时候会执行init函数
- 导入一个包会先初始化变量,然后执行init函数
- init函数先执行,在执行main函数

```go
// 导包绝对路径，从项目根目录开始
import "awesomeProject/8包/utils"

package main

import (
	"awesomeProject1/utils"					 		// 绝对路径（导入的时候直接导入包，不需要到文件）
	"awesomeProject1/utils/timeutils"		 // 目录中嵌套目录这样导入
	 										 							// 相同包中的文件不需要导入
)
func main() {
	utils.Count()
	timeutils.PrintTime()
}
```

```go
// 一个文件或者一个包中可以写多个init函数,先写的先调用
// 一个包中的多个文件中的init函数也是按照顺序的,按照文件名排序
// 对于不同包下:没有依赖,则按导入的顺序执行,有依赖则最后依赖的最先初始化
// 存在依赖的包之间不能循环导入
// 一个包可以被多个包import,但是只能初始化一次
func init(){
	fmt.Println("test init.....")
}

func init(){
	fmt.Println("另一个init函数....")
}
```

###### 2.包的注意点

```go
// _表示只执行里面的init函数,不调用其他方法,
// 因为普通的导入,如果导入了包不调用里面的函数则会报错
import (
	_ "pk1"
)

// 导入的时候前面加一个点,代表访问包中的函数时,可以省略包的名字,直接 Print Println即可
import (
	. "fmt"
)
```

###### 3.导入使用别名

```go
package main
import (
	// 导入的时候起别名，别名为u1 u2
	u1 "awesomeProject1/utils"
	u2 "awesomeProject1/utils/timeutils"
)

func main() {
	// 使用的时候，使用别名
	u1.Count()
	u2.PrintTime()
}
```

###### 4导出结构体和使用结构体

```go
// 1porson目录下的person文件
package person
// 结构体大写也代表可以被外部访问
type Person struct {
	// 字段名大写,也代表外部可以访问
	Name string
	Age int
	Sex string
}


// 2.main包中导入并使用
package main
import (
	// 导入结构体所在的包
	"awesomeProject1/person"
	"fmt"
)

func main() {
	// 使用包中的结构体
	p1 := person.Person{"王二狗",20,"男"}
	fmt.Println(p1)
}
```

###### 5.init函数

```go
// init函数不能有参数,不能有返回值
func init(){
	fmt.Println("init函数...")
}
```

###### 6.导入外部包

```go
// 1.先下载包,会下载到gopath中的src下
go get github.com/go-sql-driver/mysql

// 2.使用包
package main

import (
	// 导入外部文件
	"database/sql"
	"fmt"
	// 我们下载的包只需要执行init函数即可
	_ "github.com/go-sql-driver/mysql"
)

func main() {
	// 这个sql是database/sql
	db, err := sql.Open("mysql","root:qwe123!@#@tcp(127.0.0.1:3306)/go?charset=utf8")
	if err != nil{
		fmt.Println("错误信息",err)
	}
	fmt.Println("链接成功", db)
}
```

###### time包

```go
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func main() {
	// 1.获取现在的时间
	t1 := time.Now()
	fmt.Printf("%T\n", t1)
	fmt.Println(t1)

	// 2.获取指定的时间,arg:年 月 日 时 分 秒 纳秒 本地时间
	t2 := time.Date(2008,1,2,12,12,12,12,time.Local)
	fmt.Println(t2)

	// 3.时间转字符串(这里必须写这个时间，代表将t1转成这种格式，这个时间是go诞生的时间)
	s1 := t1.Format("2006年1月2日 15:04:05")
	fmt.Println(s1)

	s2 := t1.Format("2006/1/2")
	fmt.Println(s2)

	// 4.字符串转时间
	s3 := "1999年09月09日"
	t3, err := time.Parse("2006年01月02日",s3)
	if err != nil{
		fmt.Println("error", err)
		return
	}
	fmt.Println(t3)

	// Date函数可以获取三个返回值，年月日
	fmt.Println(t3.Date())

	// Clock()可以获取时分秒
	fmt.Println(t3.Clock())

	// 可以单独获取年月日时分秒
	fmt.Println(t3.Year())
	fmt.Println(t3.Day())
	fmt.Println(t3.Second())

	// 时间戳(秒的时间戳)
	t4 := time.Now()
	fmt.Println(t4.Unix())

	// 时间戳(纳秒的时间戳)
	fmt.Println(t4.UnixNano())

	// t4的时间加上一天
	t5 := t4.Add(24*time.Hour)
	fmt.Println(t5)

	// 求t5和t1的间隔
	fmt.Println(t5.Sub(t1))

	// sleep当前线程休眠3秒钟
	time.Sleep(3 * time.Second)
	fmt.Println("3秒钟过后...")

	// sleep [1-10的随机秒数], 需要将int转成duration类型
	rand.Seed(time.Now().UnixNano())
	randNum := rand.Intn(10) + 1
	time.Sleep(time.Duration(randNum) * time.Second)
}
```

###### file文件信息

```go
package main

import (
	"fmt"
	"os"
)

func main() {

	// os.Stat，获取文件信息
	fileInfo, err := os.Stat("./nice.txt")
	if err != nil{
		fmt.Println("error:", err)
	}

	// 打印文件信息
	fmt.Println(fileInfo)

	// 打印文件名
	fmt.Println(fileInfo.Name())
	// 打印文件大小 32字节
	fmt.Println(fileInfo.Size())
	// 是否是目录
	fmt.Println(fileInfo.IsDir())
	// 修改时间
	fmt.Println(fileInfo.ModTime())
	// 权限
	fmt.Println(fileInfo.Mode())
}
```

###### 文件1

```go
package main

import (
	"fmt"
	"os"
	"path"
	"path/filepath"
)

func main() {
	/**
	文件操作:
		相对路径: 从当前工程开始  . 和 .. 也可以使用
		绝对路径: 从根目录开始
	 */
	fileName1 := "./a.txt"
	fileName2 := "C:\\Users\\Administrator\\go\\src\\awesomeProject\\10文件操作\\a.txt"
	// 打印上面的路径是否是绝对路径
	fmt.Println(filepath.IsAbs(fileName1))
	// 获取绝对路径
	fmt.Println(filepath.Abs(fileName1))

	// 获取(绝对路径的文件的)父目录,使用path下的join函数, ..代表上一级目录
	fmt.Println(path.Join(fileName2, ".."))

	// 创建目录, 第二个参数为权限 os.ModePerm为0777
	err := os.Mkdir("./aa", os.ModePerm)
	if err != nil{
		fmt.Println(err)
		return
	}
	fmt.Println("文件创建成功.....")

	// 递归创建目录
	err1 := os.MkdirAll("./bb/cc", os.ModePerm)
	if err != nil{
		fmt.Println(err1)
		return
	}
	fmt.Println("递归文件夹创建成功...")
}
```

###### 文件创建

```go
package main

import (
	"fmt"
	"os"
)

func main() {
  // 文件创建os.Create()
  file1, err1 := os.Create("./aa/go.txt")
	if err1 != nil{
		fmt.Println("失败", err1)
	}
	fmt.Println(file1)

}
```

###### 打开文件

```go
package main

import (
	"fmt"
	"os"
)

func main() {

	// 1.open是只读的
	// file, err := os.Open("./nice.txt")

	// 2.openFile可以指定读写, arg1:文件名字, arg2:文件可读写, arg3:如果文件不存在,创建文件(文件的权限)
	file, err := os.OpenFile("./nice.txt", os.O_RDONLY | os.O_WRONLY, os.ModePerm)
	if err != nil{
		fmt.Println("err: ", err)
		return
	}
	fmt.Println(file)

	// 3.关闭文件
	file.Close()
}
```

###### 删除文件

```go
func main() {
	// remove可以删除文件,也可以删除目录
  err := os.Remove("./nice.txt")
	if err != nil{
		fmt.Println("err:", err)
		return
	}
	fmt.Println("删除文件成功....")
}
```

###### 删除目录以及目录下的内容(递归删除)

```go
func main() {
	err := os.RemoveAll("./bb")
	if err != nil{
		fmt.Println("err:", err)
	}
	fmt.Println("删除目录成功....")
}
```

######  io操作(输入输出)

```go
package main

import (
	"fmt"
	"os"
)

// 这里的IO操作相当于IO流
func main() {
	// 1.打开文件
	file, err := os.Open("./nice.txt")
	if err != nil{
		fmt.Println("err:", err)
		return
	}
	// 3.关闭文件(通过defer让他最后执行)
	defer file.Close()

	// 2.读取数据(创建byte数组,读取数据)
	bs := make([]byte,4,4)
	n ,err := file.Read(bs)
	// n为读取的长度
	fmt.Println(n)
	// bs为读取的数据,byte需要转成string
	fmt.Println(string(bs))

	// 第二次读取(会往后读取)
	_, err1 := file.Read(bs)
	fmt.Println(err1)
	fmt.Println(string(bs))
}
```

###### 循环读取数据

```go
func main() {
	// 1.打开文件
	file, err := os.Open("./nice.txt")
	if err != nil {
		fmt.Println("err")
	}

	// 2.defer关闭文件
	defer  file.Close()

	// 3.读取数据(10个字节的读,开发中可以用1024)
	bs := make([]byte, 10, 10)
	n := -1
	for{
		n, err = file.Read(bs)
		fmt.Println(string(bs))

		// 如果n的长度等于0或者is.EOF则文件读取结束,break退出循环
		if n==0 || err == io.EOF{
			fmt.Println("读取到文件的末尾,结束读取...")
			break
		}
	}
}
```

###### io写操作

```go
func main() {
	// 1.打开文件
	file, err := os.OpenFile("./app.txt",os.O_CREATE | os.O_WRONLY | os.O_APPEND, os.ModePerm)
	if err != nil{
		fmt.Println("err", err)
		return
	}

	// 4.关闭文件
	defer file.Close()

	// 2.写数据(字节写入，会变成字符ABCDE)
	bs := []byte{65,66,67,68,69}
	n, err := file.Write(bs)
	fmt.Println(n)
	HandleErr(err)

	// 3.直接写出字符串
	file.WriteString("\nHello Word")
}

func HandleErr(err error){
	if err != nil{
		log.Fatal(err)
	}
}
```

###### 复制文件

```go
package main

import (
	"fmt"
	"io"
	"os"
)

func main() {
	// 1.定义源文件和目标文件
	srcFile := "./app.txt"
	descFile := "./acc.txt"

	// 2.调用方法
	total, err := CopyFile(srcFile, descFile)
	fmt.Println(total, err)
}

// copy文件的方法,用于通过io操作实现文件的拷贝,返回值是拷贝的总数量(字节为单位)
func CopyFile(srcFile, descFile string)(int, error){
	file1, err := os.Open(srcFile)
	if err != nil {
		return 0, err
	}
	file2, err := os.OpenFile(descFile, os.O_WRONLY | os.O_CREATE, os.ModePerm)
	if err != nil{
		return 0,err
	}

	defer file1.Close()
	defer file2.Close()

	// 读写
	bs := make([]byte, 1024, 1024)
	total := 0
	n := -1				// 读取的数据量
	for{
		n, err = file1.Read(bs)
		if err == io.EOF || n == 0{
			fmt.Println("拷贝完毕,退出循环..")
			return total ,nil
		}else if err != nil{
			fmt.Println("报错了...")
			return total, err
		}
		total += n
		file2.Write(bs[:n])
	}
	return total, nil
}
```

