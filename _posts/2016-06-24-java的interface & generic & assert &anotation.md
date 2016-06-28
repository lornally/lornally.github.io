### java的接口(interface)和范型(generic)

> 这是一个open的话题, 我会持续去写.

interface能解决的问题, 为啥还要范型? 

1. 恐怕是有返回值的情况才需要范型吧?
2. 把参数返回可好?
3. 参数不会被改变, 那么一定要声明final, 这样可以有益于线程安全.

之前对于泛型的理解有偏颇了.

1. 静态方法可以有泛型参数, 但是, 不可以有泛型返回值. 换句话说, 静态域不能有泛型类型. 

2. 可以用静态方法配合泛型通配符搞定null判断. 比如:
   ```java
   public static boolean hasNulls(Pair<?> p){
   return p.getfirst()==null || p.getsecond()==null;
   }  //这个写法, 比指定T参数的写法, 更简洁明了.
   ```

3. 泛型避免了强制类型转换, 增强了类型安全. 

   ```java
   public static <T extends Camparable<? super T> T min(T[] a){
     
   }
   ```

4. 泛型不能创建数组, 不可以用来实例化, 比如:

   ```java
   pair<String>[] table= new pair<>[19]; //这个错了. 如果这样是对的, 那么这个数组可以装任何东西. 无法保证类型安全. 这个错在后半段, 不能创建, 声明是ok的.
   pair<String>[] table; //这个声明没有任何问题.
   T nt=new T(); //这个绝对错了, 泛型不能new. 不能实例化. 
   T nt=T.class.newInstance(); //这样, 用反射实例化, 也不行. T.class本身就不合法.
   public E get(int n){return (E)elements[n];} //这个正确的.elements本身是个object数组, 当做E[]使用.
   ```

5. 通配符捕获机制:

   ```java
   public static void swap(pair<?> p);//注意这个不是泛型的.
   public static <T> void swaphelper(pair<T> p){
     //在这里可以用类型T声明变量, 从而交换pair的两个元素.
   }//当然这个地方可以不这么实现, 可以实现一个泛型的swap.
   //但是, 某些情况, 必须要这样实现. 比如一定是一个pair<?>穿进来作为参数.
   ```

   ​

### 断言assert是啥用处呢?

```java
assert iv2!=null; //我们可以断言iv2非空, 如果它空了, 那么久会抛出一个异常, 并终止程序.
```

### 恍然大悟, final的用处

1. 声明一个属性,  声明为public final, 
2. 配合这个声明, 在构造函数赋值, 
3. 然后, 其他代码就只能读取了. 这个就对了. 
4. 我的imageNurl就是这么弄得.

### anotation注解

