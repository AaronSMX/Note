# Kotlin_Day02笔记



# for循环和foreach循环

**用来遍历字符串\数组\集合**

关键字:for(a in str) \ for((index,c) in str.withIndex())

```kotlin
fun main(args: Array<String>) {
    val str  = "abcd"
    /*---------------------------- for循环 ----------------------------*/
    for (a in str) {						//通过str.for回车生成
        println(a)
    }
    for ((index,c) in str.withIndex()) {	//输出角标和对应的字符,用$替换角标和字符
        println("index=$index c=$c")
    }

    /*---------------------------- foreach ----------------------------*/
    str.forEach {							//通过str.foreach回车生成
        println(it)
    }
    str.forEachIndexed { index, c ->		//通过str.forEachIndexed回车生成
        println("index=$index c=$c")
    }
}
```

**continue和break:只能用在for循环中不能用在foreach中**

**continue跳过本次循环,break结束全部循环**



###在标签处返回

在嵌套for循环中使用,在for前加标签tag@,在break时@tag

```kotlin
 tag@for (c1 in str1) {
        tag3@for (c2 in str2) {
            // b 2  后面的元素不需要再打印了
            println("$c1 $c2")
            if(c1=='b'&&c2=='2'){
                break@tag
            }
        }
    }
```



# while和do while循环

```kotlin
fun main(args: Array<String>) {
    //1到100
    var a= 101
    while (a<=100){
        println(a)
        a++
    }
    /*---------------------------- do while ----------------------------*/
    do {
        println(a)
        a++
    }while (a<=100)
}
```



# 区间

**三种区间的定义的方式;可用for和foreach遍历区间**

关键字:   .. \数据类型Range( , )\ .rangeTo()\for\foreach

```kotlin
fun main(args: Array<String>) {
    /*---------------------------- 定义1到100 ----------------------------*/
    val range1 = 1..100
    //1 until 100  [1,100)
    val range2 = IntRange(1,100)
    val range3 = 1.rangeTo(100)
    /*---------------------------- 长整形区间 ----------------------------*/
    val range4 = 1L..100L
    val range5 = LongRange(1L,100L)
    val range6 = 1L.rangeTo(100L)
    /*---------------------------- 字符区间 ----------------------------*/
    val range7 = 'a'..'z'
    val range8 = CharRange('a','z')
    val range9 = 'a'.rangeTo('z')

}
```

**反向区间和区间的反转**

关键字:downTo\reversed\for(i in range step 数字)

```kotlin
fun main(args: Array<String>) {
    /*---------------------------- 反向区间 ----------------------------*/
    //定义100到1的区间
    val range = 100 downTo 1
    range.forEach {
        println(it)
    }
    /*---------------------------- 区间反转 ----------------------------*/
    val range1 = 1..100
    val range2 = range1.reversed()
    range2.forEach {
        println(it)
    }
  	//跃步区间:等差数列的区间
    for (i in range2 step 5) {
        println(i)
    }
}
```



# 数组

**两种数组的初始化方式**

关键字:

arrayOf:可以存相同或不同数据类型的元素

数据类型Array(元素个数):可按规定个数初始化一个元素

**遍历数组用for\foreach**

```kotlin
fun main(args: Array<String>) {
    /*---------------------------- 定义数组并赋值 ----------------------------*/
    //张三  李四  王五
    val arr = arrayOf("张三","李四","王五")
    //10 20 30
    val arr1 = arrayOf(10,20,30)
    //a b c
    val arr2 = arrayOf('a','b','c')
    //"张三"  10  'a'
    val arr3 = arrayOf("张三",10,'a')
    /*---------------------------- 8中基本数组类型数组 ----------------------------*/
    //保存年纪信息
    val arr4 = IntArray(10)//new int[10]
    val arr5 = IntArray(10){	//把数组里面每一个元素都初始化为30
        30
    }

//    BooleanArray
//    ByteArray
//    ShortArray
//    CharArray
//    FloatArray
//    DoubleArray
//    LongArray
//    StringArray//不能用
}
```

**数组元素的修改**

关键字:set(角标,要修改的值)\array[角标]=要修改的值

```kotlin
fun main(args: Array<String>) {
    val array = arrayOf(1,2,3,4,5)
    //把3修改成9
    array[2] = 6		//方式一

    array.set(2,9)		//方式二

}
```

**数组元素角标的查找**

**如果没找到返回-1**

关键字:indexof\lastindexof\indexofFirst{布尔类型表达式}\indexofLast{布尔类型表达式}

```kotlin
fun main(args: Array<String>) {
    val array = arrayOf("张三","李四","张四","王五","张三","赵六")
   
  //查找第一个”张三”角标
    val index1 = array.indexOf("张三")
    println(index1)
   
  //查找最后一个”张三”角标
    val index2 = array.lastIndexOf("张三")
    println(index2)

    /*---------------------------- 高阶函数实现 ----------------------------*/
    //查找第一个姓”张”的人的角标
  	val index3 = array.indexOfFirst {
        it.startsWith("张")
   	}
    println(index3)
  
    //查找最后一个姓”张”的人的角标
    val index4 = array.indexOfLast { it.startsWith("张") }
    println(index4)

    val index5 = array.indexOfFirst { it=="张三" }
    println(index5)
}
```



# when表达式

**基本运用:**

```kotlin
fun main(args: Array<String>) {
   
    val age = 6
    
    val result = todo(age)
    println(result)
}

/**
 * 判断当前年纪做什么事情
 */
fun todo(age:Int):String{
    when(age){
        7-> return "开始上小学"

        12->{
            return "开始上中学"
        }
        15->{
            return "开始上高中"
        }
        18->{
            return "开始上大学"
        }
        else->{
            return "开始上社会大学"
        }
    }
}
```

**when表达式分支可以支持区间**

**简单的when表达式通过java中的switch语句实现**

**复杂的when表达式通过java中的if else语句实现**

关键字:in 区间\is 数据类型

```kotlin
/**
 * 判断当前年纪做什么事情
 */
fun todo1(age:Int):String{
    when(age){
        //在区间里用in关键字
        in 1..6-> return "没有上小学"
        7-> return "开始上小学"
        in 8..11->return "在上小学"
        12,13->return "开始上小学"
        13,14->return "正在上中学"
        15,16->return "开始上高中"
        16,17->return "正在上高中"
        18-> return "开始上大学"
        in 19..22->return "正在上大学"
      	is Int->return"输入的是整型"
        else->return "开始上社会大学"
    }
}
```

**when表达式也可不带参数,在分支中执行判断条件**

```kotlin
fun todo1(age:Int):String{
    when{
        age==7->{
            return "开始上小学"
        }
```

**when表达式如果有返回值可将return前提到when前,执行最后一行的语句**

```kotlin
fun todo(age:Int):String{
    return when(age){			//return可前提到when前
        7-> {
            println("找到了分支")
            10
             "开始上小学"		//return最后一行的语句
        }
```



# 函数表达式

**函数体只有一行代码  可以省略{} 省略return 用=连接,同时kotlin会自动推断返回的数据类型**

```kotlin
fun main(args: Array<String>) {
    var a = 10
    var b = 20
    println(add(a, b))
}
fun add(a:Int,b:Int) =  a+b
```

**函数变量:把匿名函数当数据变量一样初始化**

```kotlin
fun main(args: Array<String>) {
    var a = 10
    var b = 20
    val padd:(Int,Int)->Int = {a,b->a+b}//padd为函数变量.匿名函数:lambda表达式
    println(padd(10, 20))
}
```

**函数的引用:通过定义变量来引用函数**

关键字: ::

```kotlin
fun main(args: Array<String>) {
    var a = 10
    var b = 20
    val padd= ::add					//通过::引用add函数
    println(padd(10, 20))
    println(padd?.invoke(20, 30))  //可以处理函数变量为空的情况下调用
}
fun add(a:Int,b:Int):Int{
    return a+b
}
```

**可变参数:只要是参数列表中规定的参数,可以无限传入该类型的值**

关键字:vararg

```kotlin
fun main(args: Array<String>) {
    var a = 10
    var b = 20
    var c = 30
    //a+b+c
    val result = add(a,b,c,10,20,30,40,50)
    println(result)
}

/**
 * 只要是Int数据类型  无论你传递多少个我都能求他们的和  可变参数
 */
fun add(vararg a:Int):Int{
    var result = 0
    a.forEach {			//求和
        result += it
    }
    return result
}
```



# 异常

**kotlin不检查编译时异常,只检查运行时异常**

关键字:try{}catch(e:...Exception){}finally{}

```kotlin
fun main(args: Array<String>) {
    var a = 10
    var b = 0

    //异常处理
    try {
        a / b
    } catch (e: ArithmeticException) {
        println("捕获到异常")
    }finally {
        println("最终要执行的代码")
    }
```



# 递归

**实质是在函数中调用自己**

```kotlin
/**
 * 求n的阶乘
 */
fun fact(n:Int):Int{
        if(n==1){
            return 1
        }else{
            return n* fact(n-1)		//在函数中调用自己这个函数
        }
}
```

```kotlin
//斐波那契数列  1 1 2 3 5 8 13 21 34 55
//第四斐波那契数列=第三个斐波那契数列+第二个斐波那契数列
//第n个   第n-2个  + 第n-1个
/**
 * 求第n个斐波那契数列
 */
fun fibonacci(n:Int):Int{
    if(n==1||n==2){
        return 1
    }else{
        return fibonacci(n-2)+ fibonacci(n-1)	//在函数中调用自己
    }
}
```

**迭代:用循环重复计算**

```kotlin
/**
 * 求1到n的和  通过迭代的方式求和
 */
fun sum1(n: Int): Int {//kotlin里面参数是固定  不能修改
    var result = 0
    var copyN = n
    while (copyN > 0) {			//通过循环累加
        result += copyN
        copyN--
    }
    return result
}
```

**递归和迭代的对比:**

常见的需求:通过迭代和递归都可以解决 复杂的问题用递归更容易实现

StackOverflowError 递归如果递归的层级比较深容易栈内存溢出,比如:递归10万次

递归:优点:逻辑比较简单  容易实现  缺点:容易栈内存溢出(内存开销比较大)

迭代:优点:内存开销小 缺点:抽象出数学模型

**综合二者的优点引出尾递归优化:**

关键字:tailrec

```kotlin
fun main(args: Array<String>) {
    val result = sum2(100000)
    println(result)
}
tailrec fun sum2(n:Int,result:Int=0):Int{		//将result设置为默认参数
    if(n==1){
        return result+1
    }else{
        return sum2(n-1,result+n)				//调用自身之后做了其他操作
    }
}
```



# 面向对象

**创建类和创建对象调用对象的属性和方法**

```kotlin
fun main(args: Array<String>) {
    val girl = Girl()		//创建对象
    println(girl.name)		//调用对象的属性
    println(girl.age)
    girl.greeting()			//调用对象的方法

    val rect = Rect()
    println(rect.long)
    println(rect.width)
}
class Rect{					//创建类
    //静态属性
    var long:Int = 100
    var width:Int = 100
}
class Girl{
    //静态属性
    var name:String = "李四"
    var age:Int = 20
    //动态行为
    fun greeting(){
        println("hello  你好")
    }
}
```

