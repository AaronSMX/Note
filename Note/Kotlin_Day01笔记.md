# Kotlin和Java基本语法结构的对比

关键字:main回车

```java
//Java
public static void main(String[] args){
    语句体;
}
```

```kotlin
//kotlin
fun main(args: Array<String>) {
	println("hello")
}
```



# 初始化基本数据类型

**可变变量:var(编程过程中可重复赋值修改)**

**不可变变量:val(初始化后不可在编程过程中修改再赋值)**

```kotlin
fun main(args: Array<String>) {
    //定义Boolean类型变量
    var b:Boolean = false
    var byte:Byte = 10
    var int:Int =  20
    var long:Long = 40L
    var char:Char = 'a'
    var double:Double = 1.1234567
    var float:Float = 1.1234567F
    var short:Short = 10
}
```

**智能推断:直接给变量赋值,kotlin会自动推断其数据类型,并且一旦推断是一种数据类型,不得在后续更改**

```kotlin
fun main(args: Array<String>) {
    var int=10
    int=5
    int=1.2			//报错:kotlin已在之前智能推断处int为整型,在此处不可用浮点型赋值
    val str="dfs"
    val double=2.3424

}
```

**用基本数据类型.MAX_VALUE和基本数据类型.MIN_VALUE查看基本数据类型的取值范围**

关键字:.MAX_VALUE \ .MIN_VALUE

```kotlin
fun main(args: Array<String>) {
    val intmax=Int.MAX_VALUE
    val intmin=Int.MIN_VALUE
}
```

**用BigDecimal("长型小数")输出长型小数,BigDecimal译为精确小数**

关键字:BigDecimal

```kotlin
fun main(args: Array<String>) {
    val bigDecimal=BigDecimal("3.13141412312312412413123124141313132")
    println(bigDecimal)
}
```

**用数据类型.to数据类型() 实现不同数据类型之间的转换**

关键字:.to数据类型

```kotlin
fun main(args: Array<String>) {
    val str="23424"
    val int=str.toInt()
    println(int)
}
```

## 字符串的一些重要函数调用

**初始化普通单行字符串和模板性多行字符串:**

关键字:val str=""""""

```kotlin
fun main(args: Array<String>) {
    val str="jfsolfjs"  //初始化普通字符串
    
    val str1="""       //初始化模板性字符串:怎么写怎么输出 
       中国
       广东
       深圳
    """
}
```

**删除字符串的空格:**

关键字:trim\trimMargin\trimIndent

```kotlin
fun main(args: Array<String>) {
    /*---------------------------- 普通字符串删除空格 ----------------------------*/
    val str = "  张三   "
    val newStr = str.trim()	//删除字符串中所有的空格
//    println(newStr)
    /*---------------------------- 原样输出字符串 ----------------------------*/
    val str2 = """
        /张三
        /李四
        /王五
    """.trimMargin("/")		//删除/和每行文字之前的空格
    
    val str3="""
        张三
        李四
        王五
    """.trimIndent()		//删除每行文字之前的空格
    println(str2)
}
```

**字符串值的比较和地址值的比较:**

关键字:== ;.equals();===

```kotlin
fun main(args: Array<String>) {
    val str1 = "abc"
    val str2 = String(charArrayOf('a','b','c'))
    
  	//equals  比较值  true
    println(str1.equals(str2))
    
  	//== 比较的也是值
    println(str1 == str2)

    //=== 比较地址值 false
    println(str1 === str2)
}
```

**字符串的切割:**

关键字:split("","")

```kotlin
fun main(args: Array<String>) {
    val str = "张三.李四-王五"
    //多条件切割
    val result = str.split(".","-")
    println(result)		//输出的是字符串数组[张三,李四,王五]在切割处,
}
```

**字符串的截取:**

关键字:substring\substringBefore\substringBeforeLast\substringAfter\substringAfterLast

```kotlin
fun main(args: Array<String>) {
    val path = "/Users/yole/kotlin-book/chapter.adoc"
  
    //获取前6个字符
	//println(path.substring(0, 6))
    println(path.substring(0..5))//0到5
  
    //把第一个r之前的字符截取
    println(path.substringBefore("r"))
    //把最后一个r之前的字符截取
    println(path.substringBeforeLast("r"))
    //把第一个r之后的字符截取
    println(path.substringAfter("r"))
    //把最后一个r之后的字符截取
    println(path.substringAfterLast("r"))
}
```



# 二元元组和三元元组

**不用像数组\集合那样要先创建对象再能往里面存数据,元组可直接定义和返回多个值** 

关键字:Pair\A to B\pair.first\pair.second\

Triple( , , )\triple.first\triple.second\triple.third

```kotlin
fun main(args: Array<String>) {
    //定义二元元组 姓名  年纪
	val pair = Pair<String,Int>("张三",20)	//初始化二元元组的第一种方式
  
    val pair = "张三" to 20					//初始化二元元组的第二种方式
  
	println(pair.first)					//用元组名.第几获取元组的元素
	println(pair.second)

    //三元元组
    val triple = Triple<String,Int,String>("李四",20,"15456678")//初始化三元元组的方式
    println(triple.first)					//获取三元元组的元素
    println(triple.second)
    println(triple.third)
}
```



# 空值处理

**在数据类型初始化时系统默认数据类型为非空类型,即不能赋值null**

**可空类型:**可以赋值null,格式是在赋值时在赋值符号前加?**数据类型?=null**

**关闭空检查:** **在调用函数前加!!**告诉编译器不要检查了,我一定不为空,但如果为空会报错,不建议使用

**空安全调用符: 在调用函数.前加?**   如果不为空则正常调用函数,但如果为空则输出null

**Elvis操作符:** **在调用函数后加?:值**  如果为空则输出自定义的值如-1(提示为空),如果不为空则正常调用函数

```kotlin
fun main(args: Array<String>) {
    val str: String? = null		//初始化可空类型

	str!!.toInt()				//关闭空检查
  
	str?.toInt()				//空安全调用符,如果为空,输出null

    val b:Int = str?.toInt()?:-1	//Elvis操作符,如果str为空则输出-1
}
```



# 人机交互

**向电脑输入数据,电脑处理完数据并输出**

关键字:readLine\空安全调用符\Elvis操作符

```kotlin
fun main(args: Array<String>) {
    var m:Int
    var n:Int
    
    m = readLine()?.toInt()?:-1		//从控制台输入m和n让电脑读取并转换成int类型
    n = readLine()?.toInt()?:-1

    println("m+n=" + (m + n))
}
```



# 自定义函数

##函数基本定义

关键字:

fun 函数名(参数变量名:参数变量数据类型):返回值类型{语句体}**有参有返回函数**

fun 函数名(){}**无参无返回**

```kotlin
fun main(args: Array<String>) {
//    sayHello("张三")
    println(getLength("张三"))
}
//无参无返回值
fun sayHello(){//返回值
    println("hello")
}
//有参无返回值
fun sayHello(name:String){
    println("hello "+name)
}
//有参有返回值
fun getLength(name:String):Int{
    return name.length
}
//无参有返回值
fun get():String{
    return "hello"
}
```

## 顶层函数和嵌套函数(概念理解)

**函数式编程:函数是一等公民,函数可以独立于对象单独存在**

**嵌套函数:在函数中嵌套函数**

```kotlin
fun main(args: Array<String>) {
    //嵌套函数
    fun sayHello(){
        println("hello")
    }
    sayHello()
}
```

**顶层函数:函数之间并列**

```kotlin
fun main(args: Array<String>) {
    
}
//顶层函数
fun haha(){
   
}
fun hehe(){
    
}
```

## 字符串模板函数

```kotlin
//定义一个字符串模板函数,参数列表中的参数是要替换的字符串模板,返回值是String类型
//在字符串中插入${参数列表中的参数}作为要替换的字符串
//在主函数中调用字符串模板函数
```

```kotlin
fun main(args: Array<String>) {
    val str= stringmodel("张三")
    println(str)
}
fun stringmodel(name:String):String{
    val result="你好${name}"
    return result
}
```

**除了替换字符串,{}中还能调用函数:**

```kotlin
fun main(args: Array<String>) {
    println(stringmodel())
}
fun stringmodel():String{
    val result="你好${hello()}"
    return result
}
fun hello():String{
    return "李四"
}
```

**默认参数和具名参数:在函数的参数列表中直接赋值的是默认参数,通过在调用函数时输入的参数是具名参数**

```kotlin
fun sendRequest(path:String,method:String="GET"){//GET POST 默认参数
    println("请求路径=${path},method=${method}")
}
```

# 条件控制语句if

```kotlin
fun max(a: Int, b: Int): Int {
    return if (a > b) {		//return 可以放在 if前面
         a
    } else {
         b
    }
}
```

```kotlin
fun max(a: Int, b: Int): Int {
    return if (a > b) a else b	//如果只有一行代码,最终可以精简成这样
}
```

**kotlin没有三元运算符**



