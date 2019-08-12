## Golang
http://play.golang.org -----可以分享代码给别人的平台
1. 通道    
    通道是一种数据结构，可以让goroutine之间进行安全的数据通信。并发最难的地方就在于确保其他并行运行的进程，线程或goroutine不会意外修改用户的数据。当不同的的线程在没有同步保护的情况下修改同一个数据时，就会发生在灾难。其他语言中，如果使用全局变量或者共享内存，必须使用复杂的锁规则来防止对同一个变量的不同步修改。通道提供了一种新的模式，从而保证并发修改时的数据是安全的，通道这一模式保证同一时刻只会有一个goroutine修改数据。通道用于在几个运行的goroutine之间发送数据。
2. Golang语言的类型系统    
    >Golang开发者使用组合设计模式，只要将一个类型嵌入到另一个类型就能复用所有的功能。另外Golang语言还具有独特的接口实现机制，允许用户对行为进行建模，而不是对类型进行建模。在golang中不需要声明某个类型实现了某个接口，编译器会判断一个类型的实例是否符合正在使用的接口。    

    > golang接口对一组行为建模：接口用于描述类型的行为。如果一个类型的实例实现了一个接口，意味着这个实例可以执行一组特定的行为。你甚至不需要去声明这个实例实现了哪个接口，只需要实现这组行为就好。其他的语言吧这样的特性叫做鸭子类型，Go语言中，如果一个类型实现了一个接口的所有方法，那么这个类型的实例就可以存储在这个接口类型的实例中，不需要额外声明。
3. hellow world
    ```Golang
    package main　　 ●――――Go程序都组织成包。
    import "fmt" 　　●――――import语句用于导入外部代码。标准库中的fmt包用于格式化并输出数据。
    func main() {　　●――――像C语言一样，main函数是程序执行的入口。
        fmt.Println("Hello world!")
    }
    ```
4. import     
    ```Golang
        import (
        "log"
        "os"
        _ "github.com/goinaction/code/chapter2/sample/matchers"
        "github.com/goinaction/code/chapter2/sample/search"
        ) 
    在第四行法相导入matchers包的时候，在导入路径前面又一个下划线。这个技术是为了让golang语言对包做初始化操作，但是并不使用包里的标识符，Go编译器不允许声明导入某个包却不使用。下划线让编译器接受这类导入，并且调用对应包内的所有代码文件里定义的init函数。
    ```
5. init函数    
    程序中的每个代码文件里的init函数都会在main函数执行之前被调用。
6. 在Golang中，当代码导入了一个包时，程序可以直接访问这个包中任意一个公开的标识符。这些标识符以大写字母开头。以小写字母开头的标识符是不公开的，不能被其他包中的代码直接访问。
7. 关于零值     
在Go语言中，所有变量都被初始化为其零值。对于数值类型，零值是0；对于字符串类型，零值是空字符串；对于布尔类型，零值是false；对于指针，零值是nil。对于引用类型来说，所引用的底层数据结构会被初始化为对应的零值。但是被声明为其零值的引用类型的变量，会返回nil作为其值。
8. 错误处理    
    Golang引入了3个关键字用于标准的错误处理流程，这三个关键字分别是defer，panic和recover。
9. 匿名函数和闭包    
    Go语言支持常规的匿名函数和闭包，比如下列代码就定义了一个名为f的匿名函数，开发者可以随意对该匿名函数变量进行传递和调用：
    ```golang
    f := func(x, y int) int {
        return x + y
    }
    ```
    闭包是可以包含自由变量的代码块，这些变量不在这个代码块内或者任何全局上下文中定义，而是在定义代码块的环境中定义。要执行的代码块为自由变量提供绑定的计算环境（作用域）；
    ```golang
    package main
    import "fmt"
    func main() {
        var j int =5 
        a := func() (func()){
            var i int =10
            return func() {
                fmt.Println("1,j:%d\n",i,j)
            }
        }()
        a()
        j* =2 
        a()
    }
    程序输出：
    i, j: 10, 5
    i, j: 10, 10
    ```

10. 并发机制    
    通过在函数调用前使用关键字go，我们既可以让该函数已goroutine方式执行，goroutine是一种比线程更加轻盈的，更省资源的协程。Golang语言通过系统的线程来多路派遣这些函数的执行，使得每个用go关键字执行的函数可以运行成为一个单位协程。当一个协程阻塞的时候，调度器就会自动把其他协程安排到另外的线程中去执行，从而实现了程序无等待并行化运行。而且调度的开销非常小，一颗CPU调度的规模不下于每秒百万次，这使得我们能够创建大量的goroutine，从而可以很轻松地编写高并发程序，达到我们想要的目的。    
    <p>---------------------------------------------------------------<p>
    Go语言实现了CSP（通信顺序进程，Communicating Sequential Process）模型来作为goroutine间的推荐通信方式。在CSP模型中，一个并发系统由若干并行运行的顺序进程组成，每个进程不能对其他进程的变量赋值。进程之间只能通过一对通信原语实现协作。Go语言用channel（通道）这个概念来轻巧地实现了CSP模型。channel的使用方式比较接近Unix系统中的管道（pipe）概念，可以方便地进行跨goroutine的通信。
    <p>---------------------------------------------------------------<p>
    由于一个进程内的创建的所有goroutine运行在一个内存地址空间中，因此如果不同的goroutine不得不去访问共享的内存变量，访问前应该先获取相应的读写锁，Go语言标准库中的sync包提供了完备的读写锁功能。
    如例：goroutine和channel使用

    ```Golang
    package main
    import "fmt"
    func sum(values [] int,resultChan chan int){
        sum :=0 
        for _,value := range values {
            sum += value
        }
        resultChan <- sum
    }

    func main() {
        values = [] int{1,2,3,4,5,6,7,8,9}
        resultChan := make(chan int,2)
        go sum(values[:len(values)/2],resultChan)
        go sum(values[len(values)/2:],resultChan)

        sum1,sum2 :=<-resultChan,<-resultChan
        fmt.Println("Result :",sum1,sum2,sum1+sum2)
    }
    ```
11. 反射    
    反射最常用的使用场景是做对象的序列化。例如，Go语言标准库的encoding/json、encoding/xml、encoding/gob、encoding/binary等包就大量依赖于反射功能来实现。
    ```Golang
    package main
    import (
        "fmt"
        "reflect"
    )
    type Brid struct{
        Name string 
        LifeExpectance int
    }
    func (b *Brid) Fly(){
        fmt.Println("I am flying ....")
    }
    func main() {
        sparrow := &Brid{"Sparrow",3}
        s :=reflect.ValueOf(sparrow).Elem()
        typeOfT := s.Type
        for i :=0; i<s.NumField(); i++ {
            f := s.Field(i)
            fmt.Printf("%d: %s %s = %v\n", i, typeOfT.Field(i).Name, f.Type(), f.Interface())
        }
    }

    输出：
        Name string = Sparrow
        LifeExpectance int = 3
    ```
12. 语言的交互性    
    由于go和c之间的天生联系，重用现有C模块的功能，被叫做Cgo。Cgo既有语言特性，同时也是一个工具的名称。在go代码中，可以按照Cgo的特定语法混合编写C语言代码。然后Cgo工具可以将这些混合的C代码提取并生成对于C功能的调用包装代码。
    ```Golang
    package main
    import "C"
    import "unsafe"
    func main() {
        cstr :=C.CString("Hellow,world")
        C.puts(cstr)
        C.free(unsafe.Pointer(cstr))
    }
    ```
## 第一个golang代码  

```golang
package main
import "fmt"
func main() {
    fmt.Println("Hello world 你好 世界！")
}
```
每个Go源代码文件的开头都是一个package声明，表示该go代码所属的包。包是go语言最基本的分发单位，也是工程管理中依赖关系的体现。要生成Go可执行程序，必须建立一个名为main的包，并且在该包中包含一个名为main的函数。go语言的main函数不能带参数，也不能定义返回值。命令行传入的参数在os.Args变量中保存起来。
<p>--------------------------------------------------------------------</p>
所有的go函数以关键字func开头。一个常规的函数定义包含以下部分：   

    func 函数名 （参数列表）（返回值列表）{
        函数体
    }

1. 变量     
对于纯粹的变量声明，go语言引入了关键字var，而类型信息放在变量名之后：

    ```Golang
    var v1 int = 1
    var v2 string

    var v3 [10] int

    // 数组

    var v4 [] int

    // 数组切片

    var v5 struct {

    f int

    }

    var v6 * int

    // 指针

    var v7 map[ string] int

    // map，key为string类型，value为int类型

    var v8 func(a int) int
    ```
2. 变量初始化和赋值      
    ```golang
    //正确的初始化方式
    var v1 int = 10 
    var v2 = 10 
    v3 := 10
    ```
    出现在：=左侧的变量不应该是已经被声明过的，否则会导致编译错误，比如：
    ```golang
    var i int
    i := 10
    会发生如下的编译错误
    no new variables on left side of :=

    //变量赋值
    var v1 int 
    v1 = 10 
    ```
    go语言中提供了多重赋值功能，比如下面交换 i 和 j 变量的语句：
    ```golang
    i, j = j, i
    ```
3. 常量定义    
    通过const关键字。      
    ```golang
    const zero = 0.0//无类型浮点常量
    const a，b，c = 3， 4， "foo"
    ```
    go的常量定义可以限定常量类型，但不是必须的。如果定义常量的时候没有指定类型，那么他与字面常量一样，是无类型常量。常量定义的右值也可以是一个在编译运行期间的常量表达式

4. 复数类型    
    复数实际上是由两个实数构成，一个表示实部一个表示虚部。对于一个复数z=complex（x，y），可以通过Go语言内置函数real（z）获得该复数的实部，也就是x，通过image（z）获取该复数的虚部。也就是y。
5. 字符串     
    ```golang
    var str string
    str = "Hello world" // 字符串赋值
    ch := str[0]
    字符串的内容可以用类似于数组下标的方式获取，但与数组不同的是字符串的内容不能在初始化后被修改
    ```
6. 两种输出语句的区别     
    Println : 可以打印出字符串，和变量    
    Printf : 只可以打印出格式化的字符串,可以输出字符串类型的变量，不可以输出整形变量和整形
7. 字符串操作     
    字符串链接：str1+str2   
    取字符：str1[0]
8. 字符串遍历    
    go语言支持两种方式遍历字符串。一种是以字节数组的方式遍历：
    ```golang
    str := “Hello,世界“
    n := len(str)
    for i:=0 ;i<n ;i++ {
        ch := str[i] // 依据下标取字符串中的字符，类型为byte
        fmt.Println(i, ch)
    }

    程序输出：    
    0 72
    1 101
    2 108
    3 108
    4 111
    5 44
    6 32
    7 228
    8 184
    9 150
    10 231
    11 149
    12 140

    可以看出这个字符串的长度为13。尽管从直观上来说这个字符串只有9个字符，这是因为每个中文字符在UTF-8中占3个字节，而不是1个字节。
    ```
    另一种是以Unicode字符遍历：
    ```golang
    str := "Hello,世界"
    for i, ch := range str {
    fmt.Println(i, ch)//ch的类型为rune
    }

    输出结果：      
    0 72
    1 101
    2 108
    3 108
    4 111
    5 44
    6 32
    7 19990
    10 30028
    以Unicode字符方式遍历时，每个字符的类型是rune，而不是byte。在Go语言中支持两个字符类型，一个是byte（实际上是uint8的别名），代表UTF-8字符串的单个字节的值；另一个是rune，代表单个Unicode字符。出于简化语言的考虑，Go语言的多数API都假设字符串为UTF-8编码。
    ```
9. 数组     
    9.1 数组的声明     
    ```golang
    [32] byte
    // 长度为32的数组，每个元素为一个字节
    [2*N] struct { x, y int32 } // 复杂类型数组
    [1000]* float64
    // 指针数组
    [3][5] int
    // 二维数组
    [2][2][2] float64
    // 等同于[2]([2]([2]float64))
    ```
    9.2 数组访问    
    （1）使用数组下标    
    （2）使用range，遍历容器中的元素。数组也是range的支持范围。
    ```golang
    for i, v := range array {
        fmt.Println("Array element[", i, "]=", v)
    }
    从上面的例子可以知道，range具有两个返回值，第一个返回值是元素的数组下标，第二个返回值是元素的值。    
    需要注意的是，在go语言中数组是一个值类型，所有值类型变量在赋值和作为参数传递时都将产生一次复制动作。如果将数组作为函数的参数类型，则在函数调用时该参数将发生数据复制。因此，在函数体中无法修改传入的数组的内容，因为函数内操作的只是所传入数组的一个副本。
    ```
10. 数组切片    
    数组切片初看起来就像是一个指向数组的指针，实际上它拥有自己的数据结构，而不仅仅一个指针。数组切片的数据结构可以抽象为以下3个变量：
    <ul>
        <li>一个指向原生数组的指针
        <li>数组切片中的元素个数
        <li>数组切片已分配的存储空间
    </ul>
    10.1 创建数组切片      

    创建数组切片的方法总共有两种---基于数组和直接创建。            
    >基于数组创建：          
        <p>数组切片可以基于一个已存在的数组创建。数组切片可以使用数组的一部分元素或者整个数组来创建，甚至可以创建一个比所基于的数组还要大的数组切片。Go语言支持用myArray[first:last]这样的方式来基于数组生成一个数组切片</p>
    ```golang
    package main
    import "fmt"
    func main() {
        //先定义一个数组
        var myArray [10] int =[10] int{1,2,3,4,5,6,7,8,9,10}

        //基于数组创建一个数组切片  
        var mySlice [] int = myArray [:5]
        fmt.Println("Elements of myArray")

        for _,v := range myArray {
            fmt.Print(v," ")
        }

        fmt.Println("\n Elements of mySlice")

        for _,v :=range mySlice{
            fmt.Print(v," ")
        }
        fmt.Println()
    }

    运行结果：    
    Elements of myArray:

    1 2 3 4 5 6 7 8 9 10

    Elements of mySlice:

    1 2 3 4 5
    ```
    >直接创建    
    go语言提供的内置函数make()可以用于灵活的创建数组切片。下面的例子示范了直接创建数组切片的各种方法，初始值为0:
    ```golang
    //创建一个初始元素个数为5的数组切片，元素初始值为0
    mySlice1 := make([]int,5)
    //创建一个初始元素个数为5的数组切片，元素初始值为0，并预留10个元素的存贮空间
    mySlice2 := make([]int ,5,10)
    //直接创建并初始化包含5个元素的数组切片
    mySlice3 := []int{1,2,3,4,5}
    ```
    10.2 元素遍历    
    操作数组元素的所有方法都适用于数组切片。   
    10.3 动态增减元素     
    >可动态增减元素是数组切片比数组更为强大的功能。与数组相比，数组切片多了一个存储能力（capacity）的概念，即元素个数和分配的空间可以是两个不同的值。合理地设置存储能力的
    值，可以大幅降低数组切片内部重新分配内存和搬送内存块的频率，从而大大提高程序性能。  
    数组切片支持Go语言内置的cap()函数和len()函数，cap()函数返回的是数组切片分配的空间大小，而len()函数返回的是数组切片中当前所存储的元素个数。
    ```golang
    可以使用append()函数。下面的代码可以从尾端给mySlice加上3个元素，从而生成一个新的数组切片：
    mySlice = append(mySlice, 1, 2, 3)
    函数append()的第二个参数其实是一个不定参数，我们可以按自己需求添加若干个元素，甚至直接将一个数组切片追加到另一个数组切片的末尾：
    mySlice = append(mySlice, mySlice2...)
    需要注意的是，我们在第二个参数mySlice2后面加了三个点，即一个省略号，如果没有这个省略号的话，会有编译错误因为按append()的语义，从第二个参数起的所有参数都是待附加的元素。因为mySlice中的元素类型为int，所以直接传递mySlice2是行不通的。加上省略号相
    当于把mySlice2包含的所有元素打散后传入。上述调用等同于：
    mySlice = append(mySlice, 8, 9, 10)
    ```
    10.4 内容复制    
    数组切片支持go语言的另一个内置函数copy()，用于将内容从另一个数组切片复制到另一个数组切片中，如果加入的两个数组切片不一样大，就会按其中较小的那个数组切片的元素个数进行
    复制：
    ```Golang
    slice1 := [] int{1,2,3,4,5}
    slice2 := [] int{5,4,3}
    copy(slice2,slice1)//只会复制slice1的前3个元素到slice2中
    copy(slice1,slice2)//只会复制slice2的3个元素到slice1的前3个位置
    ```
11. map   
    map是一堆键值对的未排序集合，比如以身份证号作为唯一键来标识一个人的信息，则map可以定义为以下代码：
    ```golang
    package main
    import "fmt"
    // PersonInfo是一个包含个人详细信息的类型
    type PersonInfo struct {
        ID string
        Name string
        Address string
    }
    func main() {
        var personDB map[string] PersonInfo
        //往这个map里面插入几条数据
        personDB["1"] = PersonInfo{"1","jack","Room 101,..."}
        personDB["12345"] = PersonInfo{"12345", "Tom", "Room 203,..."}
        //从这个map查找键为‘123’的信息
        person, ok := personDB["1234"]
        //ok是一个返回的bool型，返回true表示找到了对应的数据
        if ok {
            fmt.Println("ok是一个返回的bool型，返回true表示找到了对应的数据")
        }else{
            fmt.Println("Did not find person with ID 1234.")
        }
    }
    ```
    map的声明基本上没有多余的元素，比如：```var myMap map[ string PersonInfo```;其中，myMap是声明的map变量名。string是键的类型，PersonInfo则死其中存放的值的类型。   
    11.1 创建   
    >我们可以使用Go 语言内置的函数make()来创建一个新的map。下面的例子创建了一个键类型为string，值类型为PersonInfo的map：```myMap = make(map[ string] PersonInfo)```,也可以在创建的时候指定该map的初始化存储能力，下面的例子创建了一个初始化存储能力为100 的map：```myMap = make(map[ string] PersonInfo, 100)```   
    创建并初始化：
    ```golang
        myMap = map[ string] PersonInfo{
    "1234": PersonInfo{"1", "Jack", "Room 101,..."},
    }
    ```
    11.2 删除    
    >Go语言提供了一个内置函数delete(),用于删除容器内的元素。```delete(myMap, "1234")```。上面的代码将从myMap中删除键为“1234”的键值对。如果“1234”这个键不存在，那么这个调用将什么都不发生，也不会有什么副作用。但是如果传入的map变量的值是nil，该调用将导致程序抛出异常（panic）。
12. 流程控制    
    >go语言支持以下几种流程控制语句：    
    <ul>
        <li>条件语句，对应的为if，else和else if；
        <li>选择语句，对应的关键字为switch，case和select
        <li>循环语句，对应的关键字为for和range
        <li>跳转语句，对应的关键字为goto
    </ul>
    条件语句中：在有返回值的函数中，不允许将最终的return语句包含在if...else...结构中       

    选择语句中：与C语言的规则相反，go语言不需要用break来明确退出一个case。可以不设定switch之后的条件表达式，在此种情况下，整个switch结构与多个if...else...的逻辑作用等同。
    ```golang
    switch {
    case 0 <= Num && Num <= 3:
        fmt.Printf("0-3")
    case 4 <= Num && Num <= 6:
        fmt.Printf("4-6")
    case 7 <= Num && Num <= 9:
        fmt.Printf("7-9")
    }
    ```
    循环语句中：for循环与C语言一样，都允许在循环条件中定义和初始化变量，唯一的区别是，Go语言不支持以逗号为间隔的多个赋值语句，必须使用平行赋值的方式来初始化多个变量。同时break可以设置中断某一处的循环。考虑到无限循环的场景，简化写法如下：
    ```golang
    //多重赋值
    a := [] int{1, 2, 3, 4, 5, 6}
    for i, j := 0, len(a) – 1; i < j; i, j = i + 1, j – 1 {
        a[i], a[j] = a[j], a[i]
    }

    //无限循环
    sum := 0
    for {
        sum++
        if sum > 100 {
            break
        }
    }

    //中断JLoop处的循环

    for j := 0; j < 5; j++ {
        for i := 0; i < 10; i++ {
            if i > 5 {
                break JLoop
            }
            fmt.Println(i)
        }

    }
    JLoop:
    // ...

    ```
13. 不定参数函数   
    不定参数是指函数传入的参数个数为不定数量。为了做到这一点，首先需要将函数定义为接受不定参数类型：
    ```golang
    func myfunc(args ...int){
        for _,arg :=range args{
            fmt.Println(arg)
        }
    }
    这单段代码的意思是myfunc()接受不定数量的参数，这些参数的类型全部都是int，所以他可以使用如下的方式来调用
    myfunc(2, 3, 4)，myfunc(1, 3, 7, 13)。
    ```
14. 错误处理error接口     
    go语言引入了一个关于错误处理的标准模式，即error接口，该接口的定义如下：
    ```Golang
    type error Interface{
        Error() string
    }
    ```
    对于大多数函数，如果要返回错误，大致上都可以定义为如下模式，将error作为多种返回值中的最后一个，但这并非是强制要求：
    ```Golang
    func Foo(param int)(n int,err error){
        //...
    }

    //调用的时候建议按照如下的方式处理错误情况
    n,err := Foo(0)
    if err != nil {
        //错误处理
    } else {
        //使用返回值n
    }
    ```
    使用自定义的error 类型，首先自定义一个用于承载错误信息的类型。
    ```golang
    type PathError struct {
        Op string 
        Path string 
        Err error 
    }
    如果是这样的话，编译器怎么知道PathError可以当作是一个error来传递呢，关键在于代码实现了Error方法：
    func (e *PathError) Error() string {
        return e.Op+" "+e.Path+":"+e.Err.Error()
    }
    
    当suscall.Stat()失败返回err时，将该err包装到一个PathError对象中返回
    func Stat (name string) (fi FileInfo,err error){
        var stat syscall.Stat_t
        err = syscall.Stat(name.&stat)
        if err != nil{
            return nil, &PathError{"stat",name,err}
        }
        return fileInfoFromStat(&stat, name), nil
    }

    //如果在处理错误时获取信息，而不满足于打印一句错误信息，那就需要用到类型转换知识了
    fi, err := os.Stat("a.txt")
    if err != nil {
        if e, ok := err.(*os.PathError); ok && e.Err != nil {
        // 获取PathError类型变量e中的其他信息并处理
        }
    }
    ```
15. defer     
    ```golang
    func CopyFile(dst ,src string)(w intu64,err error){
        srcFile ，err ：= os.Open(src)
        if err != nil {
            return 
        }
        defer srcFile.Close()
        dstFile , err := os.Create(dstFile)
        if err != nil {
            return 
        }
        defer dstFile.Close()
        return io.Copy(dstFile,srcFile)
    }
    即使其中的Copy()函数抛出异常，Go仍然就保证dstFile和srcFile会被正常关闭。如果一句话干不完的话可以使用defer后加一个匿名函数的做法：
    defer func () {
        //做复杂的工作
    }()
    ```
16. Panic和recover     
    go语言引入了两个内置函数Panic()和recover()以报告和处理运行时错误和程序中的错误
    ```golang
    func panic(interface{})
    func recover() interface{}
    ```
    当在一个函数执行过程中调用panic()函数时，正常的函数执行流程将立即终止，但函数中之前使用defer关键字延时执行的语句将正常展开执行，之后该函数将放回到调用函数，并导致逐层向上执行panic流程，直至所属的goroutine中所有正在执行的函数被终止。错误信息将被报告，包括在调用panic()函数时传入的参数，这个过程称为错误处理流程。从panic()的参数类型interface{}我们可以得知，该函数接收任意类型的数据，比如整型、字符串、对象等。调用方法很简单，下面为几个例子：
    

    
## 视频课
1. Golang的语言特性：
    > 静态编译语言的安全和性能，又达到了动态语言开发维护的高效率。(1)从C语言中继承了很多理念，包括表达式语法，控制结构，基础数据类型，调用参数传值，指针等等，也保留了和C语言一样的编译方式及弱化的指针。(2)引入包的概念。(3)垃圾回收机制。(4)天然并发。(5)吸收了管道通信机制，形成了Golang特有的管道channel，通过管道channel可以实现u 痛的goroutine之间的通信。(6) 函数可以返回多个值。(7) 新的创新，比如切片slice。
2. Golang代码编写到扩展名为go的文件中。通过```go build 文件名```命令对该go文件进行编译，生成exe文件，在dos命令下执行.exe文件就可以看到效果。(有些通过go run 命令直接运行相应的go文件（类似于执行一个脚本文件的形式，不规范）)
3. Golang的执行流程
    > 方式一：.go文件-> go build ->生成可执行文件 ->go run -> 结果    
    方式二： .go文件->go run -> 结果     
    两种方式的区别： （1）如果我们选择方式一，那么我们可以将可执行文件拷贝到没有go开发环境的机器上，仍然可以运行。（2）如果我们直接go run go源代码，那么如果要在另外一个机器上这么运行，也需要go开发环境，否则无法运行。（3）在编译式，编译器会将程序运行依赖的库文件包含在可执行文件中，所以可执行文件变大了很多。
4. Golang的转义字符     
    1）\t :一个制表符，通常使用他来实现对齐功能    
    2）\n :换行符   
    3) \\ :一个\   
    4) \" :一个“    
    5) \r : 一个回车   
5. 格式化    
    使用gofmt来进行格式化：```gofmt -w 文件名```