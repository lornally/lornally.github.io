```php
<?php
namespace LaravelAcademy\ModernPHP; //LaravelAcademy是个不错的laravel站点.
//php文件的开头可以声明名字空间, 这个命名空间后的所有类、接口、函数和常量都位于LaravelAcademy\ModernPHP中。
//不同的php文件可以都声明在一个空间里面. 这个比java灵活, java的空间和实际文件目录绑死了.
use Illuminate\Http\Response; //这个意思就是这个空间里面的类啊啥的都可以直接用.
use Illuminate\Http\Response as Res; //用res作为response的别名.
use func Namespace\functionName; //可以用函数: functionName.
use constant Namespace\CONST_NAME; //可以使用常数: CONST_NAME.
```

> 官方文档: 虽然任意合法的PHP代码都可以包含在命名空间中，但只有以下类型的代码受命名空间的影响，它们是：类（包括抽象类和traits）、接口、函数和常量。

