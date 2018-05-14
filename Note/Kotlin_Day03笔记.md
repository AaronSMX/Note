# Kotlin的main函数

```kotlin
fun main(args:Array<String>){语句体}
```

# 初始化一个类

**与主函数并列**

```kotlin
class 类名{
  var 变量名=...
  fun 方法名(参数列表){语句体}
}
```

**在主类中调用(get)自定义类中的属性或方法:**

创建自定义类的对象,用对象名.属性名或方法名调用

```kotlin
var 对象名=类名()
对象名.属性名或方法名
```

**在主类中修改(set)自定义类中的属性**

```kotlin
对象名.属性名=要修改的值
```

**设置只能访问(get)不能修改(set)的自定义类属性:(关键字:private)**

```kotlin
var name = "df"
private set
```

(**自定义访问器(关键字:set field value)**)给自定义类中的变量设置条件,如果变量不符合条件就输出原定义的属性值

```kotlin
/*如果设置的桌子价格超过150就按原价输出*/

fun main(args: Array<String>) {
    val table=Table()
    table.price=160
    println(table.price)
}
class Table{
    var price=100
    set(value) {
        if (value<150){
            field=value
        }
    }
}
```



# 有参构造的类(关键字:init constructor this)

**基本格式:**

```kotlin
class 类名(num:Int,name:String){
  var num=0			//在自定义类中还要初始化和实例化参数列表中的参数
  var name=""
  init{
    this.num=num
    this.name=name
  }
}
```

**参数列表中带有var和val的格式:**

```kotlin
class 类名(var num:Int,var name:String){
  init{			//可省略初始化参数列表中参数的步骤
    this.num=num
    this.name=name
  }
}
```

**次构函数:在自定义类中与主构造函数重名并且参数列表中有更多参数的构造函数**

先执行主构函数中的语句体再执行次构函数中的语句体

```java
/*Java中的主构和次构函数*/
class Dats {
    private  String phone;
    private String name;
    private int age;
    public Dats(String name,int age){
        this.name = name;
        this.age = age;
        System.out.println("执行了初始化");
    }
    public Dats(String name,int age,String phone){
        this(name, age);
        System.out.println("执行了次构");
        this.phone = phone;
    }
```

```kotlin
/*kotlin中的主构和次构函数*/
fun main(args: Array<String>) {
    val persons= Person7("占地方",23,"12314")
}
class Person7(var name:String,var age:Int){
    init {
        println("执行了主构函数")
    }
    constructor(name: String,age: Int,phone:String):this(name,age){
        println("执行了次构函数")
    }
}
```



# 封装

**隐藏内部实现的细节,只保留功能接口:在功能类中之定义功能实现的函数,在测试类中只能调用功能类中的函数,无法修改其属性或功能函数**

```kotlin
/*功能类:站在生产商的角度*/
class WashMachine(var brand: String, var l: Int) {
    var mode = 0

    fun openDoor() {
        println("打开洗衣机门...")
    }

    fun closeDoor() {
        println("关闭洗衣机门...")
    }

    fun start() {
        when (mode) {
            0 -> {
                println("请选择模式")
            }
            1 -> {
                println("开始放水...")
                println("水放满了...")
                println("开始洗衣服...")
                println("洗衣服模式为轻柔")
                setMotorSpeed(1000)
                println("衣服洗好了...")
            }
            2 -> {
                println("开始放水...")
                println("水放满了...")
                println("开始洗衣服...")
                println("洗衣服模式为狂柔")
                setMotorSpeed(10000)
                println("衣服洗好了...")
            }
            else->{
                println("模式设置错误")
            }
        }

    }

    private fun setMotorSpeed(speed:Int){
        println("当前转速为$speed")
    }

    fun selectMode(mode: Int) {
        this.mode = mode
    }
}
```

```kotlin
/*测试类:站在用户的角度*/
fun main(args: Array<String>) {
    val machine = WashMachine("海尔",12)
    machine.openDoor()
    println("放入牛仔裤")
    machine.closeDoor()
    machine.mode = 2
    machine.start()
}
```

# 继承(关键字:open,override)

**基本格式:**

```kotlin
open class 父类名{		//所有字段不加open,默认为final状态
  open 初始化变量
  open 初始化函数
}
class 子类名:父类名(){
  override 重写父类变量
  override 重写父类函数
}
```

**案例:**

```kotlin
/*父亲叫赵五,56岁,喜欢喝白酒;儿子赵四,23岁,喜欢喝劲酒,打印输出儿子的属性和方法*/
fun main(args: Array<String>) {
    val son=Son()
    println(son.name)
    println(son.age)
    son.drink()
}
open class Father{
    open var name = "赵五"
    open var age = 56
    open fun drink(){
        println("喜欢喝白酒")
    }
}
class Son:Father(){
    override var name="赵四"
    override var age = 23
    override fun drink() {
        println("喜欢喝劲酒")
    }
}
```



# 多态 抽象类 接口 智能类型转换

**初始化抽象类的基本格式:(关键字:abstract,override)**

```kotlin
abstract class Person {
    abstract var name: String
    abstract var age: Int
    fun eat() {			//抽象类中可以初始化具象方法,该方法只可调用
        println("吃饭")
    }
}
```

**用具体的子类继承抽象类重写属性和函数:**

```kotlin
class Zhangyu : Person() {
    override var name = "张宇"
    override var age = 23
}
```

## 接口

**接口的基本格式:**

```kotlin
interface Code {
 	var num:Int		//只声明属性类型
  	fun eat()		//只声明方法名
}
```

**接口只能声明属性的类型和方法名,在接口中写的具体方法不会实现,在调用的类中都要重写**

```kotlin
interface Code {
    var phonenum: Int
    fun coding() {		//该方法不会在创建的类中调用该接口时实现
        println("会编程")
    }
}
```

**调用接口:**

```kotlin
class Zhangyu:Code{
  override var phonenum=23131
  override fun eat(){
    println("吃饭")
  }
}
```

## 多态和智能类型转换

```kotlin
fun main(args: Array<String>) {
    val zhangyu: Person = Zhangyu()//通过父类创建子类对象(多态的表现形式)
    println(zhangyu.name)
    println(zhangyu.age)
    if (zhangyu is Zhangyu) {	//智能类型转换;如果通过父类创建了子类的对象,而父类中没有子										类中的方法,要将对象进行转换
        zhangyu.coding()
        zhangyu.baking()
    }
}
```



# 嵌套类和外部类(关键字:inner)

**嵌套类是静态的,和外部的类没关系,因此创建嵌套类的对象时可以用外部类.嵌套类()的方式创建,就像调用静态工具类的函数;嵌套类无法访问外部类的属性和函数**

```kotlin
fun main (args:Array<String>){
  val inclass = Outclass.Inclass()
}
class Outclass{
  var name="张三"
  class Inclass{
    println(Outclass.name)	//无法访问外部类的属性
  }
}
```

**内部类的创建格式:在嵌套类前加inner**

**内部类创建对象的格式:val 对象名=外部类().内部类()**

**内部类可以访问外部类的属性**

```kotlin
fun main(args: Array<String>) {
    val oc=OC().IC()		//创建内部类对象
    oc.eat()
}
class OC{
    var age = 10
    inner class IC{		//内部类的初始化格式
        fun eat(){
            println("吃东$age 西")	//内部类可调用外部类的属性
        }

    }
}
```

**${this@外部类名.外部类属性}可在内外部类中有相同属性时调用外部类的属性**

```kotlin
class OutClass1{
    var name = "张三"
    inner class InnerClass1{
        var name="李四"
        fun sleep(){
            println("你好${this@OutClass1.name}")
        }
    }
}
```

# 泛型

## 泛型类

**泛型类的定义:**

```kotlin
open class Box<T>(var thing:T)				//T代表任意类的泛型
class FruitBox(thing:Fruit):Box<Fruit>(thing)//放水果类
class AppleBox(thing:Apple):Box<Apple>(thing)//放苹果类
class SonBox<T>(thing:T):Box<T>(thing)		//不知道具体放什么东西,放泛型类

abstract class Fruit	//新建要传入泛型的类
class Apple:Fruit()		
```

**使用泛型类:**

```kotlin
fun main(args:Array<String>){
  val box1 =Box("张三")	//可以是任意数据类型
  println(box1.thing)
  val apple = Apple()	//创建要使用泛型类的对象
  val box2 = Box(apple)		//传入要使用泛型类的对象
  println(box2.thing)
}
```

##泛型函数

**基本格式:**

```kotlin
fun<T>函数名(thing:T){}
```

## 泛型上限

**给传入的泛型T设置上限,使其只能传入上限类及其子类**

```kotlin
fun main(args: Array<String>) {
    val thing=Snack()
    val chocobox=ChocolateBox(thing)	//报错:snack是choco的父类,超过上限不能传参
}
open class Box<T>(var thing:T)
class ChocolateBox<T:Choco>(thing:T):Box<T>(thing)	//给泛型设置上限

abstract class Thing
open class Snack:Thing()
class Choco:Snack()
```

## 泛型擦除(关键字:inline; reified)

**错误重现:**

```kotlin
fun main(args: Array<String>) {
    val box1=Box<Int>(3)
    val box2=Box<String>("df")
    val name1=box1.javaClass.name
    val name2=box2.javaClass.name
    println(name1)
    println(name2)			//获取的两个泛型的class名应该不同,但现在显示相同
}
open class Box<T>(var thing:T)
```

**通过反射获取正确的泛型class名:**

****

```kotlin
fun main(args: Array<String>) {
    fanshe(10)							//调用泛型函数传入泛型
    fanshe("fdsf")
}
open class Box<T>(var thing:T)
inline fun<reified T>fanshe(thing:T){	//添加关键字初始化泛型函数
    val name = T::class.java.name		//引用函数
    println(name)
}
```

## 泛型类型投射

**在泛型集合中在泛型类前添加out关键字,可传入其及其子类;添加in关键字,可传入其及其父类**

**功能同在java中的?extends和super关键字**

```kotlin
fun main(args: Array<String>) {
    val list= ArrayList<Animal>()
    setAnimalList(list)
}
fun setAnimalList(list:ArrayList<out Animal>){	//可传入它及其子类作为参数;改成in可传入													其及其父类
    println(list.size)
}
open class Box<T>(var thing:T)
abstract class Thing
open class Animal:Thing()
class Monkey:Animal()
class Rabbit:Animal()
```

**星号投射:让泛型集合可以接收任意类型的泛型**

```kotlin
fun main(args: Array<String>) {
    val arr = ArrayList<Short>()	//泛型集合内可以传任意泛型类
    println(setList(arr))
}
fun setList(list:ArrayList<*>){	//*代表可以不定泛型类
    println(list.size)
}
```