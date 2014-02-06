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

### Numeric (class)

* 定义了`bytes` `kilobytes` `megabytes` `gigabytes` `terabytes` `petabytes` `exabytes`来计算字节容量
* 对`Float, Fixnum, Bignum, BigDecimal`的默认的`to_s`函数进行改写，可输出`:phone,:currency,:percentage,:delimited,:rounded,:human_size,:human`格式
* 定义了`seconds` `minutes` `hours` `days` `weeks` `ago` `since`可方便地对数字进行时间的转换

### Object (class)

* 定义了`blank?` `present?`，并对`NilClass,FalseClass,TrueClass,Array,Hash,String,Numeric`定义了`blank?`方法来进行判断
* 定义了`duplicable?`方法，`Object`设置为`true`，`NilClass,FalseClass,TrueClass,Symbol,Numeric`设置为`false`
* 为`Object` `Array` `Hash`定义了`deep_dup`函数以进行深度复制
* 定义了`instance_values` `instance_variable_names`分别返回实例变量的哈希表和数组
* 为`Object, Array, FalseClass, Float, Hash, Integer, NilClass, String, TrueClass`定义了`to_json`

```ruby
[Object, Array, FalseClass, Float, Hash, Integer, NilClass, String, TrueClass].each do |klass|
  klass.class_eval do
    # Dumps object in JSON (JavaScript Object Notation). See www.json.org for more info.
    def to_json(options = nil)
      ActiveSupport::JSON.encode(self, options)
    end
  end
end
```

* 定义了`to_param`，`Object`返回`to_s`，`NilClass,TrueClass,FalseClass`返回`self`，`Array`和`Hash`也进行了定义
* 为`Object` `Array` `Hash`定义了`to_query`，返回URL请求字符串
* 为`Object` `NilClass`定义了`try` `try!`方法，若调用方法存在则执行，反之返回`nil`，`NilClass`全部返回`nil`
* 定义了`with_options`可对有相同哈希参数的情况进行代码简化

### Range (class)

* 重新定义了`to_s`方法，可根据需要对输出字符串进行格式化
* 定义了`each` `step`方法，除了时间对象不可迭代外，其余均可进行迭代操作
* 重新定义了`include?`方法，可对`Range`是否包含进行判断
* 定义了`overlaps?`来判断两个范围是否有重叠

### String (class)

* 定义了`at` `from` `to` `first` `last`可对字符串进行截取操作
* 定义了`to_time` `to_date` `to_datetime`可将字符串转换为时间/日期
* 定义了`exclude?`检查字符串是否不包含另外的字符串
* 定义了`squish` `squish!`移除多余的空格并清除换行，定义了`truncate`对字符串进行省略截取
* 定义了`indent` `indent!`对字符串进行缩进操作
* 定义了`pluralize` `singularize`进行单/复数转换
* 定义了`constantize` `safe_constantize`将字符串转换为对象
* 定义了`camelize` `titleize` `underscore` `dasherize` `demodulize` `deconstantize` `parameterize` `tableize` `classify` `humanize` `foreign_key`对字符串进行变形操作
* 定义了`inquiry`将字符串转换为`ActiveSupport::StringInquirer`对象
* 定义了`mb_chars` `is_utf8?`可对非英文的其他字符进行正常的操作
* 定义了`html_safe`函数，将字符串转换为`ActiveSupport::SafeBuffer`对象
* 定义了`strip_heredoc`可对`Here Document`进行缩进裁剪，使其对其
* 定义了`in_time_zone`可将字符串转换为`TimeWithZone`

### Time (class)

基本内容和`Date` `DateTime`类似

### Enumerable (module)

* 定义了`sum` `index_by` `many?` `exclude?`方法，对`Enumerable`对象进行迭代操作

### Marshal (class)

* 重新对`load`进行了定义，捕捉错误并抛出特定异常

## ActiveSupport::Concern (module)

主要用于模块包含和扩展，并计算了依赖情况

```ruby
module M
  def self.included(base)
    base.extend ClassMethods
    base.class_eval do
      scope :disabled, -> { where(disabled: true) }
    end
  end

  module ClassMethods
    ...
  end
end
```
可用`ActiveSupport::Concern`来简化代码
```ruby
require 'active_support/concern'

module M
  extend ActiveSupport::Concern

  included do
    scope :disabled, -> { where(disabled: true) }
  end

  module ClassMethods
    ...
  end
end
```

## ActiveSupport::Autoload (module)

使Rails更便于加载所有模块和代码，只需要在模块或者类里加入`extend ActiveSupport::Autoload`即可使用`autoload`

另外该模块还包括`eager_autoload`和`eager_load!`方法

## ActiveSupport::Logger (class)

继承自`Logger`，定义了`broadcast`类方法，用于把信息发送到其他日志

### LoggerSilence (module)

* 定义了`silence`方法，用于调用时不输出信息到日志

## Lazy load hooks

* 定义了`ActiveSupport.on_load` `ActiveSupport.run_load_hooks`两个函数，用于迟缓加载
