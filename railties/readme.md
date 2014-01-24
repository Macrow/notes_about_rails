# railtie 负责将所有Rails组件组合绑定粘合到一起

## Rails启动过程

`config.ru` -> `environment.rb` -> `application.rb` -> `boot.rb`

### boot.rb
加载Bundler，加载Gemfile所指定的所有gem

### application.rb
1. 通过`require 'rails/all'`加载Rails环境
2. Bundler加载运行环境（development, test, production）
3. 定义Rails工程，并进行初始化设置

```ruby
module NewApp
  class Application < Rails::Application
    ...
  end
end
```

### environment.rb
通过`NewApp::Application.initialize!`初始化应用程序

### config.ru
通过`run Rails.application`将应用程序交给Rack并运行

## module Rails

封装了`application`,`cache`,`logger`三个参数

#### `application`为`Rails::Application`实例

```ruby
rails new new_app
cd new_app
rails c
Rails.application.class  # => NewApp::Application
Rails.application.is_a?(NewApp::Application)  # => true
```

TODO: new_app::Application的实例是什么时候传递给Rails.application的？

#### `cache`为`ActiveSupport::Cache::FileStore`实例

```ruby
Rails.cache.class  # => ActiveSupport::Cache::FileStore
```

#### `cache`为`ActiveSupport::Logger`实例

```ruby
Rails.logger.class  # => ActiveSupport::Logger
```

### Rails::Railtie (class)
* include `Rails::Initializable`
* 为继承自Railtie的所有class或者module（Rails::Engine和Rails::Application除外）包含了`Railtie::Configurable`
* 运用下面的代码，`Rails::Railtie`记录了所有子类

```ruby
def subclasses
  @subclasses ||= []
end

def inherited(base)
  unless base.abstract_railtie?
    base.send(:include, Railtie::Configurable)
    subclasses << base
  end
end
```

* 用config为每一个Railtie实例储存了`Railtie::Configuration`实例，用于存储配置

```ruby
class MyRailtie < Rails::Railtie
  # Customize the ORM
  config.app_generators.orm :my_railtie_orm

  # Add a to_prepare block which is executed once in production
  # and before each request in development
  config.to_prepare do
    MyRailtie.setup!
  end
end
```

* 可通过`rake_tasks`，`console`，`runner`，`generators`加入自己的内容

```ruby
class MyRailtie < Rails::Railtie
  rake_tasks do
    load "path/to/my_railtie.tasks"
  end
  
  generators do
    require "path/to/my_railtie_generator"
  end
end
```

### Rails::Railtie::Configurable (module)

封装了`instance`参数，通过`method_missing`将所有的方法转到`instance`来调用

### Rails::Engine (class)

`Rails::Engine` is a `Rails::Railtie`

当定义一个Rails::Engine子类的时候，会调用`base.send(:include, Railtie::Configurable)`

```ruby
class MyEngine < Rails::Engine
  config.autoload_paths << File.expand_path("../lib/some/path", __FILE__)
end
```

而`Rails::Railtie::Configurable`封装了`instance`参数，通过`method_missing`将所有的方法转到`instance`来调用
当调用`config`方法时，就会初始化`MyEngine`，然后转到这个实例(instance)来调用

```ruby
MyEngine.config # => MyEngine.new.config
MyEngine.config.class # => Rails::Engine::Configuration
```

所以，在定义里的config实际上就是`Rails::Engine::Configuration`的一个实例

```ruby
def inherited(base)
  unless base.abstract_railtie?
    Rails::Railtie::Configuration.eager_load_namespaces << base

    base.called_from = begin
      # Remove the line number from backtraces making sure we don't leave anything behind
      call_stack = caller.map { |p| p.sub(/:\d+.*/, '') }
      File.dirname(call_stack.detect { |p| p !~ %r[railties[\w.-]*/lib/rails|rack[\w.-]*/lib/rack] })
    end
  end

  super # go to codes below
end

def inherited(base)
  unless base.abstract_railtie?
    base.send(:include, Railtie::Configurable)
    subclasses << base
  end
end
```

#### Rails::Engine::Configuration (class)

继承自`Rails::Railtie::Configurable`，记录了Engine的middleware，generators，paths，eager_load_paths，autoload_once_paths，autoload_paths

### Rails::Application (class)

`Rails::Application` is a `Rails::Engine`

### Rails::Initializable (module)

包含了`Rails::Initializable::Initializer`和`Rails::Initializable::Collection`，用于存储所有初始化过程
`Rails::Initializable::Collection`里面运用拓扑排序，把所有的初始化依赖关系进行了有序排列，然后逐一加载

常用到的方法：`Rails::Initializable.initializer`

```ruby
def initializer(name, opts = {}, &blk)
  raise ArgumentError, "A block must be passed when defining an initializer" unless blk
  opts[:after] ||= initializers.last.name unless initializers.empty? || initializers.find { |i| i.name == opts[:before] }
  initializers << Initializer.new(name, nil, opts, &blk)
end
```
使用方法
```ruby
class MyRailtie < Rails::Railtie
  initializer "my_railtie.configure_rails_initialization" do
    # some initialization behavior
  end
  
  initializer "my_railtie.configure_rails_initialization" do |app|
    app.middleware.use MyRailtie::Middlewarend
  end
end
```

### Rails::Info (class)

记录加载的Ruby、Rails相关版本信息，ActiveSupport,ActiveRecord,ActionPack,ActionMailer等相关版本信息，Rack Middleware加载情况，以及App根目录，运行环境和数据库相关信息。

### Rails::InfoController (class)

提供Rails相关信息页面渲染。

### Rails::WelcomeController (class)

提供新建Rails工程默认首页渲染。

