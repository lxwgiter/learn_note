## 1、为什么使用lambda

​    Lambda 是一个 匿名函数，我们可以把 Lambda 表达式理解为是 一段可以传递的代码（将代码像数据一样进行传递）
​    使用它可以写出更简洁、更灵活的代码。作为一种更紧凑的代码风格，使Java的语言表达能力得到了提升。
​    **实际上，在其他语言中lambda表示的是匿名函数，但是在Java中lambda更像是函数式接口的匿名实现类对象。**



## 2、lambda语法

​    在Java 8 语言中引入的一种新的语法元素和操作符。这个操作符为 “->” ， 该操作符被称为 Lambda  操作符。或箭头操作符。
​    它将 Lambda 分为两个部分：
​        左侧：指定了 Lambda 表达式需要的参数列表
​        右侧：指定了 Lambda体，是抽象方法的实现逻辑，也即Lambda 表达式要执行的功能。



## 3、lambda中的类型推断

​    **Lambda表达式中无需指定类型**，程序依然可以编译，这是因为 javac 根据程序的上下文，在后台推断出了参数的类型。
​    Lambda 表达式的类型依赖于上下文环境，是由编译器推断出来的。这就是所谓的“类型推断”。



## 4、lambda的六种书写方式

语法格式一 ：无参，无返回值
​        Runnable r1=() -> {System.out.println("Hello Lambda!");};



语法格式二：Lambda需要一个参数，但是没有返回值
​        Consumer<String> com=(String str) -> {System.out.println(str);};
​   

语法格式三：数据类型可以省略，因为存在类型推断
​        Consumer<String> com=(str) -> {System.out.println(str);};
​   

语法格式四：Lambda若只需要一个参数，参数的小括号可以省略
​        Consumer<String> com=str -> {System.out.println(str);};
​    

语法格式五：Lambda需要两个或以上的参数，多条执行语句，并且可以有返回值
​        Consumer<Integer> com=(x,y)->{
​            System.out.println("实现函数式接口的方法");
​            return Integer.compare(x,y);
​        };
​    

语法格式六：当Lambda只有一条执行语句是，return和大括号均可以省略
​        Consumer<Integer> com=(x,y)->Integer.compare(x,y);



## 5、什么是函数式接口

​     只包含一个抽象方法的接口，称为 函数式接口。
​     你可以通过 Lambda 表达式来创建该接口的对象。（若 Lambda 表达式抛出一个受检异常(即：非运行时异常)，
​        那么该异常需要在目标接口的抽象方法上进行声明）。
​     我们可以在一个接口上使用 @FunctionalInterface 注解，这样做可以检查它是否是一个函数式接口。同时 javadoc
​        也会包含一条声明，说明这个接口是一个函数式接口。
​     在java.util.function包下定义了Java 8 的丰富的函数式接口



## 6、如何理解函数式接口

​     Java从诞生日起就是一直倡导“一切皆对象”，在Java里面面向对象(OOP)编程是一切。但是随着python、scala等语言的兴起和新技术的挑战，Java不得不做出调整以便支持更加广泛的技术要求，也即java不但可以支持OOP还可以支持OOF（面向函数编程）



​     在函数式编程语言当中，函数被当做一等公民对待。在将函数作为一等公民的编程语言中，Lambda表达式的类型是函数。但是在Java8中，有所不同。在Java8中，Lambda表达式是对象，而不是函数，它们必须依附于一类特别的对象类型——函数式接口。



​     简单的说，在Java8中，Lambda表达式就是一个函数式接口的实例。这就是Lambda表达式和函数式接口的关系。也就是说，只要一个对象是函数式接口的实例，那么该对象就可以用Lambda表达式来表示。



​     所以以前用匿名实现类表示的现在都可以用Lambda表达式来写



## 7、Java四大内置核心函数式接口



| 函数式接口               | 参数类型 | 返回类型 | 用途                                                         |
| ------------------------ | -------- | -------- | ------------------------------------------------------------ |
| Consumer<T>消费型接口    | T        | void     | 对类型为T的对象应用操作，包含方法：void accept(T t)          |
| Supplier<T>供给型接口    | 无       | T        | 返回类型为T的对象，包含方法：T get()                         |
| Function<T, R>函数型接口 | T        | R        | 对类型为T的对象应用操作，并返回结果。结果是R类型的对象。包含方法：R apply(T t) |
| Predicate<T>断定型接口   | T        | boolean  | 确定类型为T的对象是否满足某约束，并返回boolean 值。包含方法：boolean test(T t) |



## 8、方法引用：

​     当要传递给Lambda体的操作，已经有实现的方法了，可以使用方法引用！



​     方法引用可以看做是Lambda表达式深层次的表达。换句话说，方法引用就是Lambda表达式，也就是函数式接口的一个实例，通过方法的名字来指向一个方法，可以认为是Lambda表达式的一个语法糖。

​     要求：实现接口的抽象方法的参数列表和返回值类型，必须与方法引用的方法的参数列表和返回值类型保持一致！
​     格式：使用操作符 “::” 将类(或对象) 与 方法名分隔开来。
​     如下三种主要使用情况：
​        对象:: 实例方法名
​        类:: 静态方法名
​        类:: 实例方法----》(特殊对待)



## 9、方法引用例子：

​    对象:: 实例方法名,例如:
​        Consumer<String> con =(x) -> System.out.println(x);
​        等同于:Consumer <String> con2 = System.out :: println;



​    类:: 静态方法名,例如:
​        Comparator <Integer> com = (x,y) ->Integer.compare(x, y);
​        等同于:Comparator<Integer> com1 = Integer::compare;



​    类:: 实例方法,例如:
​        BiPredicate<String, String> bp = (x,y) -> x.equals(y);
​        等同于:BiPredicate <String,String> bp1 = String::equals;



## 10、构造器引用：

​    格式： ClassName::new
​        与函数式接口相结合，自动与函数式接口中方法兼容。可以把构造器引用赋值给定义的方法，要求构造器参数列表要与接口中抽象
​        方法的参数列表一致！且方法的返回值即为构造器对应类的对象。
​    例如:
​    Function<Integer, MyClass> fun =(n)-> new MyClass(n);
​    等同于:Function<Integer, MyClass> fun = MyClass: :new;



## 11、数组引用：

​    格式:type[] :: new
​    例如:
​    Function<Integer, Integer[]> fun =(n) -> new Integer[n];
​    等同于:Function< Integer, Integer[]> fun = Integer[] : :new;













