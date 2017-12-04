> 不可能不读文件吧? 我们就用读取文件这件事来比较各个语言的不同吧.

# groovy

> groovy的世界会比java好吗?

### 注释
```groovy
#! 注释可以在第一行, 指明启动脚本.
// /*和java一样.
```

### 变量

```groovy
def a=1
//其它和java一样
def b='B'
def c="heeeloo wooooreeeld"
def d='''多行字符
第二行
第三行'''
def e='heelo ${xxx}' //和php一致.
def regex=/.*hello world.*/  //这个是正则
def escapeSlash = /char\/forward slash/ //这个是用斜杠定义字符串, 和引号是一样的.

```

### array数组  list列表 map集合

```groovy
def list=[1,2,3,4]
def persons=[1:100,2:101,3:102]
def key="name"
def person=[(key):'admin'] //圆括号代表变量表达式
//键值对是啥? 键值对就是层次库, 就是nosql. 
//sql领先在于数学模型领先, 做到了范式, 数据上就是完美无暇的. 不会脏, 也不会阻断. 但是, 这种用表的组织方式, 天生就慢.
//目前数据准确已经不是用唯一性保证的了, 而是看权重.
//由于集合非常简单, 上面的操作就可以翻出很多花样.
person.add(1); //加一个
person<<"come on"; //又加一个
person[collect.size()]=100.0 //还是加一个, 这个属于受虐型的.
println person[collect.size()-1]//检索最后一个
println person[-2]      //索引其倒数第2个元素
person=person+5        //在集合中添加元素5
person=person-'come on'         //在集合中减去元素a(第3个)
person=person-person[0..4]   //把集合中的前5个元素去掉
person=person+['weight':25]       //添加john的体重
person.put('length',1.27)      //添加john的身高
person.father='Keller'         //添加john的父亲
println person['father']       //通过key作为下标索引
println person.length          //通过key作为成员名索引
```

### 函数

```groovy
def getDef(){
    return "hello world"
}

def printSomething(param01,param02){
    println param01+param02  //可以不写return, 默认返回最后一行的结果.
}
printSomething "hello","world" //函数调用, 可以不用括号, 用空格.
genre?.toUpperCase() //非空调用 如果genre不空才会做调用, 避免了java的null判断.
def printHello=this.&printSomething //函数是第一公民, 可以直接赋给变量(其实是函数)
def repeat(val,repeat=3){ //有默认值的参数
  for(i in 0..<repeat){
    println "This is ${i}:${val}"
  }
}
{}//闭包定义了一个作用域, 可以理解为一个匿名函数. 
//返回值也是最后一行
//参数用->定义 ->之前的都是参数, 之后的都是函数体.
def closure={
    param01,param02,param03->println param01+param02+param03
}
closure "hello","world","ok"
//闭包和函数的区别就在于, 闭包可以使用外部变量
```



### 其实在java中对象才是第一公民

> 因此groovy有很多实现绕不开这个.

1. 闭包其实是对象, 因此才能享受第一公民的待遇, 只不过groovy用编译器解决了, 看起来在groovy中函数才是第一公民, 但是,这些函数, 在运行期其实都是对象.

2. groovy 不需要: 

   (1)    不需要public修饰符

   (2)    不需要类型说明

   (3)    不需要getter/setter方法 这才是正路, eclipse自动生成是走到歪路上面了.

   (4)    不需要构造函数

   (5)    不需要return

   (6)    不需要()号

   (7)    不需要 import 主意不是自动生成import, 而是真的不用写了, 编译器生成而不是编辑器生成, 遵循了markdown哲学.

```groovy
class Person {//get set return 和构造函数其实都有的
  def name
  def age
  String toString(){//注意方法的类型String，因为我们要覆盖的方法为String类型
    "$name,$age"
  }
  def person2=new Person 'name':'gg','age':22
```

### 关于groovy的一些杂项

1. 语言的简洁性, 可以直观的用java和groovy书做对比.
2. typora对groovy的支持是令人发指的. 麻蛋, 比ide都好
3. 用sdkman安装属于自己找麻烦
4. 极客学院的教程不好, 是面向对象思路写的, 并不是函数式思路.
   1. 神奇了, 对译的官方文档
   2. 极客: http://wiki.jikexueyuan.com/project/groovy-introduction/groovy-development-kit.html
   3. 官方: http://www.groovy-lang.org/differences.html
5. groovy过多的使用了运行时, 导致运行速度较慢.

```groovy
int method(String arg) {
    return 1;
}
int method(Object arg) {
    return 2;
}
Object o = "Object";
int result = method(o); //结果是1, 因为是运行时判断o就是个string
```

代码对比:

java

```java
//java7 ARM 已经比之前手工关流简化了很多, 给之前类似的javaer默哀一下.
Path file = Paths.get("/path/to/file");
Charset charset = Charset.forName("UTF-8");
try (BufferedReader reader = Files.newBufferedReader(file, charset)) {
    String line;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }

} catch (IOException e) {
    e.printStackTrace();
}



//据说还有这样神奇的写法
import java.util.Scanner
import java.io.File
new Scanner(new File("file.txt")).useDelimiter("\\Z").next()

Scanner fileIn = new Scanner(new File(thePathToYourFile));
//然后其实还是麻烦, 要自己判断越界问题.  
```

groovy

```groovy

//groovy 闭包

new File('/path/to/file').eachLine('UTF-8') {
   println it
}

// java8 lambda
Runnable run = () -> System.out.println("Run");
list.forEach(System.out::println);


//groovy lambda 除了大括号, 没引入任何其他符号
Runnable run = { println 'run' }
list.each { println it }

//流处理, 再举3个例子
//1. 需要将某个文本文件的行放入列表中，可以这样写：

def list = new File(baseDir, 'haiku.txt').collect {it}

//2. 还可以利用 as 操作符将获得的文件内容放到一个关于行的数组中：

def array = new File(baseDir, 'haiku.txt') as String[]

//3. 将文件内容用 byte[] 放到一个字节数组中

byte[] contents = file.bytes

//4. 开一个输入流, 无需关闭, 闭包结束时会自动关闭.
new File(baseDir,'haiku.txt').withInputStream { stream ->
    // 省略的逻辑语句......
}

//5. 向文件写入字符串
new File(baseDir,'haiku.txt') << '''Into the ancient pond
A frog jumps
Water’s sound!'''

//6. 直接写入字节数组
file.bytes = [66,22,11]//这么写可以的
new File(baseDir,'data.bin').withOutputStream { stream ->//这么写也是可以的.
    // 省略的逻辑语句......
}
//groovy的文件和文本处理很厉害啊. 貌似可以完成我处理gbk文件名和文件info的目标.
```



# python

1. 无用的括号, 函数
2. 无用的: for循环

```python
with open('xxx.md') as file_o: 
  con=file_o.read()
  print(con)
```



# php

```php
$myfile = fopen("webdictionary.txt", "r") or die("Unable to open file!");
echo fread($myfile,filesize("webdictionary.txt"));
fclose($myfile);
```



# ruby

> 把 | |  换成{}, 就明了了

```ruby
#此处readlines 可以是open或者read, 但是open就需要close了.
File.readlines('dom.js').each do |line| 
	puts line
end
#更简单的方案 IO.read or File.read automatically close the file,  so there is no need to use File.open with a block. 果然还是英文靠谱
puts File.read(file_name)

#另外的方法, 据说大文件有区别
IO.foreach("testfile") {|x| print "GOT ", x }
# or
File.foreach('testfile') {|x| print "GOT", x }
```



# swift

他的思路真奇葩, 并没有使用file句柄这种东西, 咋读, 取决于你要把文件读成啥, 真牛.面向数据结构的swift 一切都回到了原点

```swift
//nsstring
let path = ""
let content = NSString(contentsOfFile: path, encoding: NSUTF8StringEncoding, error: nil)

//nsdata
let jsonPath = ''
let data = NSData.init(contentsOfFile: jsonPath!)
```



# kotlin

> 这个思路比较正常有各种reader,
> 貌似也不用处理流的打开和关闭了.
> 但是推断不如swift 例如,  
> var ins:InputStream = file.inputStream() 
> 冒号:后面还要写内容, 并且这一堆的[.]啊.

 ```kotlin
val iStr = File("kon.txt").inputStream().bufferedReader().use
{it.readText()}
println(iStr)
//另外一种方法
val fiC = this::class.java.getResource("/he.html").readText()
 ```

### 进入另一个领域 函数式functional

scheme

```scheme
(apply string (file->char_list "mydata.txt"))
;另一种写法
(foldr (lambda (x y) (string-append (string x) y)) "" (file->char_list "mydata.txt"))
;更简单
(file->lines "somefile")
```

golang

```go
package main
import ( //这个意思就是包要手动管理了?
    "fmt"
    "io/ioutil"
)
func main() {
    b, err := ioutil.ReadFile("file.txt") // just pass the file name
    if err != nil { //这个意思就是错误要显示处理了?
        fmt.Print(err)
    }
    fmt.Println(b) // print the content as 'bytes'
    str := string(b) // convert content to a 'string'
    fmt.Println(str) // print the content as a 'string'
}
```



haskell

```haskell
import System.IO  
import Control.Monad
main = do  
        let list = []
        handle <- openFile "test.txt" ReadMode
        contents <- hGetContents handle
        let singlewords = words contents
            list = f singlewords
        print list
        hClose handle   
f :: [String] -> [Int]
f = map read
```



clojure

```clojure
(slurp "/tmp/test.txt") ;read entire file
(use 'clojure.java.io) ;read line by line
(with-open [rdr (reader "/tmp/test.txt")]
  (doseq [line (line-seq rdr)]
    (println line)))

```



erlang



```erlang
file2lines(File) ->
   {ok, Bin} = file:read_file(File),
   string2lines(binary_to_list(bin), []).

readlines(FileName) -> %一种
    {ok, Data} = file:read_file(FileName),
    binary:split(Data, [<<"\n">>], [global]).

{ok, Bin} = file:read_file(Filename). %最简

read(File) -> % line by line
    case file:read_line(File) of
        {ok, Data} -> [Data | read(File)];
        eof        -> []
    end.
```

nodejs

```js
fs.readFile('/etc/passwd', function (err, data) { //异步
  if (err) throw err;
  console.log(data);
});
var text = fs.readFileSync('test.md','utf8') //同步
console.log (text)
```

scala

```scala
//没关文件
val lines = scala.io.Source.fromFile("file.txt").mkString

//关文件的写法
val source = scala.io.Source.fromFile("file.txt")
val lines = try source.mkString finally source.close()
```



### 语言决定了你的思维方式

- nodejs 异步非阻断
- swift 数据驱动
- scheme 一切都是函数
- java 目录结构决定代码结构
- groovy 闭包就是函数
- python 代码结构决定逻辑结构
- php 咋干都行
- ruby 闭包干他 或者lambda被干
- kotlin 一路...
- clojure 命令行一样的scheme
- erlang 大括号不是函数体



### 我们应该涉猎的语言

1. groovy 学习闭包
2. python 文本处理相当优雅
3. ruby 简洁 闭包和lambda可以随意切换, 理解他们其实是一样东西的两种写法.
4. swift 数据结构驱动和多参数函数
5. kotlin 静态的functional
6. nodejs 异步非阻塞闭包调用





### 更多语言

Facebook is trying something similar with languages [called Hack](https://www.wired.com/2014/03/facebook-hack/) and [D](https://www.wired.com/2014/07/d-programming-language/).

Google is exploring this ground with Go. 

Mozilla, maker of the Firefox web browser, is doing much the same with a language known as Rust.

Lattner created something called Clang

Light Table,  Light Table can do much the same thing—and do it with multiple languages, including Python, Clojure, and Javascript

R和scheme一样是lisp衍生的

### 排名

- https://www.tiobe.com/tiobe-index// 
- Stack Overflow Developer Survey
  - https://insights.stackoverflow.com/survey/2016
  - https://insights.stackoverflow.com/survey/2017

rust

f#

react

smalltalk

typescript

elixir

julia

dart

CLU

### 对比总结

1. 无非lambda和闭包两种形式, 而且他们其实还是一回事, 是一个事物的两种描述.
2. 代码生成是恶魔, 代码生成vs 编译器推断 vs 运行期解决. 
3. 运行期解决依赖问题, 会导致代码运行比较慢.
4. 只有编译器才是王道. markdown的路是最正确的路.
5. 括号也是一种罪恶, 不论是大括号, 中括号, 还是小括号. python的路是正确的.

### 再补充

perl

```perl
open( my $input_fh, "<", $input_file ) || die "Can't open $input_file: $!";
#perl的设计哲学比较烦人: 任何事都有n中方法去做.
#货吸引一堆人去用, 也是因为数学完备性, 和sql一毛一样
#但是他是有价值的, 因为他几乎尝试了所有的语法设计, 前置, 后置, 中置,不仅仅是运算符, 还有条件判断也可以这么玩, 各种花式的设计键值对, 各种花式循环, 花式if, 花式.....
#可以这么说, 所有的语言都是perl的一个子集....
#因为第一个markdown是perl写的, 那个语法看着觉得奇怪, 怎么也猜不着, 某个变量究竟是啥. 然后看了一下这货的语法, 震惊了.
#一个数组究竟是几, 取决于你怎么用它.
#perl是最炫技的, 我去, 整个是语法大全, 脑洞大的飞起来. https://qntm.org/files/perl/perl_cn.html
#随便列举两个笑点:
1. BEGIN块总是首先执行。如果你创建了多个BEGIN块（别这么做），它们将按照解释器解释它们的顺序自上而下执行。BEGIN即使出现在脚本中间（别这么做）或者脚本最后（也别这么做），它也会首先被执行。不要搞乱自然的代码执行顺序，总是把BEGIN块放在开头！


2. 内置函数-e用于测试文件是否存在。
print "what" unless -e "/usr/bin/perl";
内置函数-d用于测试文件是否是目录。
内置函数-f用于测试文件是否是普通文件。
这只是一大波形如-X的函数中的三个，其中X是某些小写或大写字母。这类函数被称作文件检测函数。请注意字母前面的减号，用Google搜索的时候，减号表示从搜索结果中排除包含这个词的结果，这样就导致很难用Google搜索文件检测函数了！用“Perl file test”来搜索就好。
 

```



