> 调优总是建立在跟踪和测试的基础上, 蒙着调不会有好结果. 当然, 调优之前先把结构理一理, 把各种warning搞一搞, 我觉得是很有必要的, 这样调优时能省好多力气. 

### 调优之前

1. 结构先重构下, 看看内聚和解耦是否好.
2. warning处理下, 可以删除冗余代码.
3. 删除不必要的中间变量, 这个很重要, 可以发现很多隐藏的bug. 比如在函数中new一个局部变量, 再赋值给对象属性, 不如直接清空对象属性, 再在对象属性中做直接的操作, 这样, 异常发生时, 也比较清晰, 对象属性也可以声明为final, 线程更安全.
4. 删除不必要的判断条件, 或者把判断条件变为系统本身就有的东西, 比如我们会置一个标志位, 判断是否layout到合适的position, 那么, 可以改为判断view的下边缘位置是否为0, 这个判断更本质, 副作用更少. 更安全, 并且无需重置和初始化.
5. 通盘考虑下, 各种功能结构应该放的位置, 可以把功能放在更合理的位置. 这个我还没有找到合适的工具. 一般就用纸和笔.

### 调优的判断, 各种性能工具

### 调优的判断, 代码工具

1. 使用debug, 不要忘了android是有debug功能的. 而且还很强大, 可以跟踪下去.

2. 使用log, 这个要用到反射机制:

   ```java
   public class mactivity extends Activity implements layoutxxx{
     /**
     *这是构建过程中的回调函数, 可以setcontentview.
     **/
     @Override
     protected void onCreate(Bundle save){
     	super.onCreate(save);
       
       setContentView(layoutmain);
       //因为从这里知道了, 调用了getwindow().xxx, 那么getwindow()究竟是啥呢?
       //我们可以使用log.
       Log.d("TAG", getWindow().getClass().getName()); 
       //log输出: D/ViewDemoActivity(3969): com.android.internal.policy.impl.PhoneWindow
       //从log判断, 我们可以看出来这里调用了phonewindow
   }
   }
   ```

   ​

### 调优考虑的方向

1. 接口化.
2. ​