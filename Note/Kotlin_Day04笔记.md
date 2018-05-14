# Kotlin_Day04笔记



# 中缀表达式

**让代码更加简洁易懂,能更贴近自然语言的表达习惯**

```kotlin
fun main(args: Array<String>) {
    val 张三 = Person()

    张三  sayHelloTo "李四"			//更贴近自然语言的表达,输出:你好李四
  
}
class Person{
    infix fun sayHelloTo(name:String){
        println("你好 $name")
    }
}
```



# 类委托

**调用被委托者的行为方法**

关键字:by

```kotlin
类委托的步骤:
//通过接口定义洗碗的能力和行为
//创建被委托人的类(大头儿子),让他拥有和实现这个能力
//创建委托人的类(小头爸爸),让他也拥有这个能力并委托给被委托人(大头儿子)
//在主类中创建委托人的对象(小头爸爸),调用洗碗的行为,让他洗碗(他会自动委托给大头儿子去洗碗)
```

```kotlin
fun main(args: Array<String>) {
    val smallHeadFather = SmallHeadFather()
    smallHeadFather.wash()			//让小头爸爸洗碗,但他委托大头儿子去洗碗了
  									//输出:大头儿子开始洗碗了
}

interface WashPower{				//定义洗碗能力
    fun wash()							//定义洗碗行为
}

class BigHeadSon:WashPower{			//创建大头儿子这个类,让他拥有洗碗能力
  									
    override fun wash() {			//大头儿子拥有洗碗能力
        println("大头儿子开始洗碗了")
    }
}

class SmallHeadFather:WashPower by BigHeadSon()	 //小头爸爸将洗碗的能力委托给大头儿子
```

**类委托的第二种实现方式:**

**在有多个被委托人的情况下,可以在创建委托人的类时定义接口参数变量,在调用时再指定被委托人**

```kotlin
fun main(args: Array<String>) {
    val smallHeadFather = SmallHeadFather(SmallHeadSon())//在调用时再指定被委托人
    smallHeadFather.wash()
}

interface WashPower{
    fun wash()
}

class SmallHeadSon:WashPower{			//委托人一:小头儿子
    override fun wash() {
        println("小头儿子开始洗碗")
    }

}

class BigHeadSon:WashPower{				//委托人二:大头儿子
    override fun wash() {
        println("大头儿子开始洗碗了")
    }
}

//在创建委托人类的时候建立参数列表,定义接口变量,只要有洗碗能力的人都能被指定为被委托人
class SmallHeadFather(var washPower: WashPower):WashPower by washPower

```

**类委托功能加强:**

**在创建委托人的类时加入语句体,重写接口参数中的方法,并在其中调用接口中的行为.在调用时输出的语句体更丰富**

```kotlin
fun main(args: Array<String>) {
    val smallHeadFather = SmallHeadFather(SmallHeadSon())
    smallHeadFather.wash()
}

interface WashPower{
    fun wash()
}

class SmallHeadSon:WashPower{
    override fun wash() {
        println("小头儿子开始洗碗")
    }

}

class BigHeadSon:WashPower{
    override fun wash() {
        println("大头儿子开始洗碗了")
    }
}

class SmallHeadFather(var washPower: WashPower):WashPower by washPower{
    override fun wash() {
        println("付给小头儿子1块钱")
        washPower.wash()			//通过接口参数.接口方法名()的方式调用洗碗行为
        println("干的很好  下次继续")
    }
}
```



# 属性委托

关键字:by

```kotlin
逻辑导图:
在主函数中 bigHeadSon.压岁钱 = 150
->在Mother类中 i接收到的值为150,保管在妈妈那儿子的钱变为100,儿子实际拿到的钱变为50
->将Mother类中getValue中的儿子实际拿到的钱(50)返回给BigHeadSon类中的压岁钱
->在主函数中调用bigHeadSon.压岁钱时输出50
```

**多敲几遍理解逻辑**

```kotlin
fun main(args: Array<String>) {
    val bigHeadSon = BigHeadSon()			
    bigHeadSon.压岁钱 = 150	//给了儿子150元
    println(bigHeadSon.压岁钱)	//存在被委托人(妈妈)那100元,自己实际拿了50元
}

class BigHeadSon{				//创建委托人(大头儿子)的类
    var 压岁钱:Int by Mother()		//将压岁钱的属性(get和set方法)委托给被委托人(妈妈)
  									//按F2定位到报错处,按alt+enter在妈妈类中重写压岁钱的										get\set方法
}

class Mother{					//创建被委托人(妈妈)的类
	var 儿子实际拿到的钱 = 0		//初始化被委托的属性
    var 保管在妈妈那儿子的钱 = 0		//初始化其它属性
   
 	operator fun getValue(bigHeadSon: BigHeadSon, property: KProperty<*>): Int {
        return 儿子实际拿到的钱
    }
  																//i为被set的值
    operator fun setValue(bigHeadSon: BigHeadSon, property: KProperty<*>, i: Int) {
        儿子实际拿到的钱 += 50			
        保管在妈妈那儿子的钱 += i-50
    }
}
```



# 惰性加载

**用的时候再加载,优化内存,此外惰性加载是线程安全的**

关键字:by lazy

```kotlin
val name1: String  by lazy {		//by lazy放到成员变量中可单独存在;只初始化一次,用val										初始化的
    println("初始化了")
    "张三"							//返回的是最后一行
}

fun main(args: Array<String>) {
    println(name1)
}
```



# 延迟加载

**用的时候再赋值 ,不赋值不能用,程序报错**

```kotlin
lateinit var name1:String
fun main(args: Array<String>) {
    name1 = "haha"
    println(name1)
}
```

**惰性加载 VS 延迟加载:**

by lazy 和  lateinit  都可以单独使用或者放到成员变量中使用

by lazy 知道具体值   用的时候再加载

lateinit  不知道具体值  后面再赋值

by lazy变量必须通过val修饰  lateinit需要通过var修饰



# 扩展函数

**不改变已有类的情况下，为类添加新的函数**

类比java中的util类,在util类中写static方法可以在其它类中用**基本数据类型的类名或类的对象名.方法名**调用工具类,不用重复写程序

String类扩展 fun String.扩展函数名,还能扩展可空类型fun String?.扩展函数名

**扩展函数可以访问当前对象里面的方法和成员变量**

```kotlin
fun main(args: Array<String>) {
    val str:String? = null
    val myIsEmpty = str.myIsEmpty()			//用类名或其对象名.方法名调用扩展函数
    println(myIsEmpty)
    val father = Father()					
    println(father.eat())
    println(father.age)
}

fun String?.myIsEmpty():Boolean{			//String类的扩展函数
    return this==null||this.length==0
}

class Father{
    var age=0								//初始化成员变量age
}

fun Father.eat():String{					//Father类的扩展函数
	age=23									//在Father类的eat方法中修改age的值为23
	var name="张三"							//这是类的方法中的局部变量,无法调用
    return "吃饭"
}
```



# 单例

**除了方法以外所有字段都是static的,适合在字段不多的时候用**

单例和工具类是两个概念,但用法很像,通过**类名.属性或方法名**调用

关键字:object

```kotlin
fun main(args: Array<String>) {
    						//用类名.属性名或方法名调用单例
    Utils.name
    Utils.sayHello()
}
							//设置成一个单例
object Utils{
    var name = "张三"

    fun sayHello(){
        println("hello")
    }
}
```



# 伴生对象

在类中用companion{}括起来的初始化属性和方法是static静态的,在其它方法中可以用**类名.属性名(方法名)**来调用

关键字:companion

```kotlin
fun main(args: Array<String>) {
    println(Person.name)				//用类名.的方式调用
    Person.eat()
}

class Person{
    companion object {					//在类中给属性和方法装上静态方法
        var name="张三"
        fun eat(){
            println("吃饭")
        }
    }
}
```

和java一样的单例



# 枚举

**基本声明\调用\遍历方式:**

```kotlin
fun main(args: Array<String>) {
    println(WEEK.Monday)			//用枚举类名.元素获取枚举元素
   	val result = WEEK.values()		//通过枚举名.value()将枚举元素放进Array集合
    result.forEach {				//通过高阶for循环遍历枚举集合
        println(it)
    }
}

enum class WEEK{					//声明枚举类
    Monday,Tuesday,Wednesday		//直接存放元素
}
```

**枚举在when表达式中的使用:**

```kotlin
fun main(args: Array<String>) {
    todo(WEEK.Wednesday)
}

fun todo(week:WEEK){			//(对象名:枚举类名)
    when(week){
        WEEK.Monday,WEEK.Tuesday-> println("on duty")	
        WEEK.Wednesday-> println("休息")
    }
}

enum class WEEK{
    Monday,Tuesday,Wednesday
}
```

**枚举加强:在枚举中定义构造函数**

**给枚举类传参,在定义元素时传参,可调用输出元素的参数**

```kotlin
fun main(args: Array<String>) {
    println(COLOR.RED.r)
    COLOR.RED.g
    COLOR.RED.b
}

enum class COLOR(var r:Int,var g:Int,var b:Int){
    RED(255,0,0),BLUE(0,0,255),GREEN(0,255,0)
}
```



# 密封类

**加强版的枚举,元素是类.和枚举类似,密封类用密封类名.元素类名调用**

```kotlin
fun hasRight(startk:NedStark):Boolean{
    return when(startk){
        is NedStark.AryaStark->true				//用密封类名.元素类名调用
        is NedStark.SansaStark->true
        is NedStark.RobStark->true
        is NedStark.BrandonStark->true
        else->false
    }
}

sealed class NedStark{							//密封类封装的是类 类是确定的
    class RobStark : NedStark()

    class SansaStark : NedStark()

    class AryaStark : NedStark()

    class BrandonStark : NedStark()
}
```



# 数据类

**视频源:**19.数据类.avi

**概念:**只保存数据,没有其它其它操作,类似java中的bean类

**用法: ** 

**定义**在初始化普通类前加个data关键字

**调用**创建数据类对象,用.参数名的方式获取数据 \对象名.component1() \ val(参数名,参数名...)=数据类名(赋值),直接打参数名获取参数

优点:省略了java在构造函数时调用的6个方法:get\set,toString,hashcode,equals,有参构造,copy

**代码:**

```kotlin
fun main(args: Array<String>) {
    val news = News("标题","简介","路径","内容")
    news.title
    news.desc

    news.component1()//第一个元素
    news.component2()//第二个元素

    //解构
    val (title,desc,imgPath,content) = News("标题","简介","路径","内容")
    println(title)
    println(desc)
}

data class News(var title:String,var desc:String,var imgPath:String,var content:String)
```

