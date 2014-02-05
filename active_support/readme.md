# ActiveSupport 包含各种基本扩展和设置

## ActiveSupport Core Ext

### Array (class)

* 定义了`from` `to` `second` `third`等函数获取队列元素
* 定义了`to_sentence` `to_formatted_s` `to_xml`对队列进行转换
* 定义了`extract_options!`来对队列获取hash参数
* 定义了`in_groups_of` `in_groups` `split`对队列进行整理和重排
* 定义了`append` `prepend`从队列头或队列尾向队列增加元素
* 定义了`wrap`来对数据进行队列包装

### Benchmark (class)

* 定义了`ms`可对事件耗时用毫秒评估，比如`Benchmark.ms { User.all }`

### BigDecimal (class)

* 定义了`to_formatted_s`对原来的`to_s`方法进行了改写

### Class (class)

* 定义了`cattr_reader` `cattr_writer` `cattr_accessor`来设置类变量
* 定义了`class_attribute`来设置类的属性，这些属性在类和继承类中独立存在互不干扰
* 定义了`superclass_delegating_accessor`来定义singleton属性
* 定义了`descendants`获取所有后继类，定义了`subclasses`获取所有继承自该类的类

### Date (class)

* 定义了`acts_like_date?`并返回`true`
* 定义了`beginning_of_week`设置或者获取星期的第一天，默认为`:monday`
* 定义了`yesterday` `tomorrow` `current`等类函数返回日期
* 定义了`ago` `since` `beginning_of_day(month,year)` `advance` `change`函数计算日期
* 定义了`Date::DATE_FORMATS`哈希表，以用来对`to_formatted_s`进行格式化，并将`to_formatted_s`定义为输出字符的默认函数
* 定义了`in_time_zone`对日期进行时区转换

### DateAndTime::Calculations (module)

顾名思义，该模块定义了一系列对日期/时间的比较和计算，`Date`和`Time`都使用了该模块

### DateTime (class)

* 定义了`acts_like_date?` `acts_like_time?`并返回`true`
* 和`Date`一样定义了一些进行计算比较的函数，比如`past?` `future?` `change` `advance`等
* 定义了`Time::DATE_FORMATS`哈希表，以用来对`to_formatted_s`进行格式化，并将`to_formatted_s`定义为输出字符的默认函数
* 定义了`in_time_zone`对日期时间进行时区转换

### File (class)

* 定义了`atomic_write`来对文件进行原子操作，可保证其他进程/线程看到完成的操作或修改

### Hash (class)

* 定义了`to_xml`将哈希表转换为xml
* 定义了`deep_merge` `deep_merge!`对哈希进行深度合并
* 定义了`except` `except!`对哈希表进行排除
* 定义了`with_indifferent_access`将哈希表转换为`HashWithIndifferentAccess`
* 定义了`(deep_)transform_keys(!)` `(deep_)stringify_keys(!)` `(deep_)symbolize_keys(!)` 对所有哈希键进行转换
* 定义了`reverse_merge` `reverse_merge!`对哈希进行保留合并
* 定义了`slice` `slice!`生成指定keys的新哈希表，定义了`extract!`去掉指定的keys的部分

### Integer (class)

* 定义了`ordinalize` `ordinal`，并运用`ActiveSupport::Inflector`对数字进行序列转化
* 定义了`multiple_of?`来计算该数字能否被除尽（不含余数）
* 定义了`month(s)` `year(s)`将数字转换为秒数，`month(s)`是按照30天计算的，`year(s)`是按照365.25天（计算了闰年）计算的

### Kernel (module)

* 定义了`silence_warnings` `enable_warnings` `with_warnings` `silence_stderr` `silence_stream`等函数，主要用于日志操作
* 重新定义了`class_eval`，将`class_eval`全部基于`singleton_class`

### Module (class)

* 定义了`alias_method_chain`对函数进行置换修改，定义了`alias_attribute`可对属性定义别名

```ruby
alias_method :foo_without_feature, :foo
alias_method :foo, :foo_with_feature
 # 等同于
alias_method_chain :foo, :feature
```

* 定义了`anonymous?`检查模块是否有名字以确定是否为匿名模块
* 定义了`attr_internal_reader(writter,accessor)`可对模块内部变量进行定义
* 定义了`mattr_reader` `mattr_writter` `mattr_accessor`来对模块变量进行定义
* 定义了`delegate`可对任意对象进行进行委派

```ruby
delegate :hello, :goodbye, to: :greeter
delegate :name, :address, to: :client, prefix: :customer
delegate :age, to: :profile, allow_nil: true
```

* 定义了`deprecate`可全局调用`ActiveSupport::Deprecation.deprecate_methods`进行弃用警告
* 定义了`parent_name`返回父模块名称，`parent`返回父模块，没有父模块则返回`Object`，`parents`返回父模块数组
* 定义了`qualified_const_defined?` `qualified_const_get` `qualified_const_set`对定义的变量进行检查
* 定义了`reachable?`检查该模块是否可以通过名字获取
* 定义了`remove_possible_method`移除所有已定义的函数，包括私有函数，`redefine_method`可重新对函数进行定义

## ActiveSupport::Autoload (module)

使Rails更便于加载所有模块和代码，只需要在模块或者类里加入`extend ActiveSupport::Autoload`即可使用`autoload`

另外该模块还包括`eager_autoload`和`eager_load!`方法