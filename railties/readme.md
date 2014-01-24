# railtie 负责将所有Rails的组件组合绑定和粘合到一起

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

### config.rb
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

### class Rails::Application

`Rails::Application` is a `Rails::Engine` is a `Rails::Railtie`

#### class Railtie

##### Railtie::Configurable



#### class Engine

#### class Application

### module Rails::Initializable

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

### Rails::Info

记录加载的Ruby、Rails相关版本信息，ActiveSupport,ActiveRecord,ActionPack,ActionMailer等相关版本信息，Rack Middleware加载情况，以及App根目录，运行环境和数据库相关信息。

### Rails::InfoController

提供Rails相关信息页面渲染。

### Rails::WelcomeController

提供新建Rails工程默认首页渲染。

