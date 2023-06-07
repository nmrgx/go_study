1.切片：拥有相同类型元素的可变长度的序列,基于数组类型做了一层封装
    使用make或直接[]T的方式进行定义
2.指针是带类型的，不同类型的指针不能进行赋值
3.new是用来初始化值类型指针的
    var a = new(int) //得到一个int类型指针a
    var b = new([3]int) //得到一个int类型数组，b依然是指针
4.make是用来初始化slice、map、chan的
5.panic和recover
    panic是代码运行时的错误，会导致程序奔溃，异常退出；可以在代码中调用panic关键字让程序异常退出，类似内核中的bug()函数
    recover将程序从panic中恢复，recover只能在defer函数中使用
6.函数和方法的区别
    函数是谁都可以调用的
    方法是只有某个特定的类型才能调用，函数指定接受者就是方法，又叫方法接收器
        func (t Type) 函数明(参数) (返回值1， 返回值2)
		相比函数的定义多指定了(t Type)，表示该方法调用者必须是Type类型实例
		注意：不可以给別的包定义的类型添加方法，可以给自定义的类型添加方法
7.结构体的骚操作
    匿名字段：结构体中某些字段没有指定字段名，只指定类型，如下：
        type student struct {
            name string
            string
            int
        }
        该写法不推荐，如果不指定字段名，不可以出现相同的类型
        实例化：
        var stu1 student {
            name: "小明"
            string: "想有个家！"
            int: "0"
        }
        访问结构体中的匿名字段：fmt.Println(stu1.string)
    结构体嵌套时：可以直接访问嵌套结构体的一个字段，使用该特性可以模拟“继承”
        type address struct {
            province string
            city string
        }
        type student struct {
            name string
            age string
            adr address
        }
        如果访问实例stu1中adr的city字段一般方式：fmt.Println(stu1.adr.city)
                                   高级的方式：fmt.Println(stu1.city)
8.json序列化和反序列化
    要求结构体字段名首字母大写，小写的话json包中就访问不了结构体变量。
    序列化：json.Marshal()和json.MarshalIndent()
    反序列化：json.Unmarshal()
    成员标签定义：结构体成员在编译期间关联的一些元信息。
9.defer和return：return x，先执行返回值=x然后执行ret指令；defer是在赋值和返回中间执行的，即先执行返回值=x，然后执行defer，最后执行ret指令。
10.接口（interface）是一种抽象的对象，关心动作不关心数据，接口有数个方法组成，定义格式如下：
    type 接口类型名1 interface {
        方法名1(参数列表1) 返回值列表1
        方法名2(参数列表2) 返回值列表2
    }
    只要一个类型实现了“方法名1”和“方法名2”，我们就认为这个类型实现了“接口类型1”这个接口，也就是说这个类型是“接口类型1”的一个变量。
    判断方法是否已经实现，方法名相同，参数和返回值只要类型和数量一样即可，不需要参数名和返回值名相同。
11.空接口：没有定义方法的接口，即interface{}类型，任何类型都是空接口的实例，可以接收任何类型
    空接口作为函数参数，可以像函数传递任何参数
    空接口作为map的值，同一个map中存放不同类型的数据
12.接口嵌入：避免在多个地方重复声明相同的方法
13.类型断言：将空接口转化成指定类型的值，使用x.(T)，其中x是类型为interface{}的变量，T表示x可能的类型（要转换的类型），通常如下使用：
    switch v := x.(type) {
    case int:
        fmt.Println("x为int类型")
    case string:
        fmt.Println("x为string类型")
    default：
        fmt.Println("x为不知道的类型")
    }
14.常量生成器iota从0开始，逐项加1，用法如下：
    type Weekday int
    const (
        Sunday weekday = iota
        Monday
        Tuesday
        Wednesday
        Thursday
        Friday
        Saturday
    )
    上面表明，Sunday的值为0，Monday的值为1，以此类推。
15.反射：可以直接获取接口值的动态类型和动态值，任何接口值都可以理解成由reflect.Type和reflect.Value两部分组成。
    应用：各种web框架，配置文件解析库、ORM框架
    reflect.Typeof()可以获取任意值的类型对象（reflect.Type）
        反射中关于类型分为两种：类型（Type）和种类（Kind）