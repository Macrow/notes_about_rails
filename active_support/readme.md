# ActiveSupport 包含各种基本扩展和设置

## ActiveSupport Core Ext

## Array

* 定义了`from` `to` `second` `third`等函数获取队列元素
* 定义了`to_sentence` `to_formatted_s` `to_xml`对队列进行转换
* 定义了`extract_options!`来对队列获取hash参数
* 定义了`in_groups_of` `in_groups` `split`对队列进行整理和重排
* 定义了`append` `prepend`从队列头或队列尾向队列增加元素
* 定义了`wrap`来对数据进行队列包装

## Benchmark

* 定义了`ms`可对事件耗时用毫秒评估，比如`Benchmark.ms { User.all }`

## BigDecimal

* 定义了`to_formatted_s`对原来的`to_s`方法进行了改写

## Class

* 定义了`cattr_reader` `cattr_writer` `cattr_accessor`来设置类变量
* 定义了`class_attribute`来设置类的属性，这些属性在类和继承类中独立存在互不干扰
* 定义了`superclass_delegating_accessor`来定义singleton属性
* 定义了`descendants`获取所有后继类，定义了`subclasses`获取所有继承自该类的类

## Date

* 定义了`acts_like_date?`并返回`true`

## ActiveSupport::Autoload

使Rails更便于加载所有模块和代码，只需要在模块或者类里加入`extend ActiveSupport::Autoload`即可使用`autoload`

另外该模块还包括`eager_autoload`和`eager_load!`方法