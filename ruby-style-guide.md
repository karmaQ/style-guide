这是一份 [Github Ruby Styleguide](https://github.com/styleguide/ruby) 的翻译。
## 代码风格
* 使用 `UTF-8` 作为源文件的编码。
* 每行代码限制在80个字符之内。
* 每个缩排层级使用两个**空格**，不要使用硬tab。
* 不要遗留行尾空格。
* 在逗号 `,` 、冒号 `:` 、分号 `;` 之后 ，`{`前后以及`}`之前添加空格。

    ```
  sum = 1 + 2
  a, b = 1, 2
  1 > 2 ? true : false; puts "Hi"
  [1, 2, 3].each { |e| puts e }
    ```
 * 在`（`，`[`之后以及`]`和`）`之前不需要空格。
  
  ```
  some（arg）.other
  [1, 2, 3].length
  ```
* `!`之后不要空格 。
* `when`和`case`同级缩进。
  
  ```
  case
  when song.name == "Misty"
    puts "Not again!"
  when song.duration > 120
    puts "Too long!"
  when Time.now.hour > 21
    puts "It's too late"
  else
    song.play
  end

  kind = case year
       when 1850..1889 then "Blues"
       when 1890..1909 then "Ragtime"
         when 1910..1929 then "New Orleans Jazz"
         when 1930..1939 then "Swing"
         when 1940..1950 then "Bebop"
         else "Jazz"
         end
  ```

* 在 `def` 中使用空行来把代码分隔成合乎逻辑的段落。
  
  ```
  def some_method
      data = initialize(options)
      data.manipulate!
      data.result
  end

  def some_method
      result
  end
  ```
    
## 文档
使用[Tomdoc]（http://tomdoc.org/）来书写文档。如此的美妙:

```
# Public: Duplicate some text an arbitrary number of times.
#
# text  - The String to be duplicated.
# count - The Integer number of times to duplicate the text.
#
# Examples
#
#   multiplex("Tom", 4)
#   # => "TomTomTomTom"
#
# Returns the duplicated String.
def multiplex(text, count)
  text * count
end
```
  
## 语法
* 使用 `def` 时，当方法接收参数时使用括号，不接受参数时，省略括号。

  ```
  def some_method
    # 此处省略方法体
  end

  def some_method_with_arguments（arg1, arg2）
    # 此处省略方法体
  end
  ```
    
* 不要使用 `for` ，除非你时分清楚为什么使用，大多数情况下应该使用迭代器。`for` 是由 `each` 实现的（所以你绕弯了），不同于 `for` 的是 - 它不会引入一个新的作用域（ `each` 会）以至于定义于其内部区块的变量在外部也是可见的。

  ```
  arr = [1, 2, 3]
  # bad
  for elem in arr do
    puts elem
  end
  
  # good
  arr.each { |elem| puts elem }
  ```
  
* 避免使用三元操作符，除非所有表达式都是极其简短的。另外，应该使用三元操作符来代替单行的 `if/then/else/end` 结构。

  ```
  # bad
  result = if some_condition then something else something_else end
  
  # good
  result = some_condition ? something : something_else
  ```
  
* 三元操作符的每个分支只写一个表达式。即不要嵌套三元操作符。对于嵌套的情况应使用 `if/else` 结构。
  
  ```
  # bad
  some_condition ? （nested_condition ? nested_something : nested_something_else） : something_else
  
  # good
  if some_condition
    nested_condition ? nested_something : nested_something_else
  else
    something_else
  end 
  ```
  
* 不要使用 `and` 和 `or` 关键字。这是不值得的。用 `&&` 和 `||` 来代替吧。
* 用 `if/unless` 来避免多行的`?:` （三元操作符）。
* 对于单行的代码主体尽量使用修饰性的 `if/unless`。

  ```
  # bad
  if some_condition
    do_something
  end
  
  # good
  do_something if some_condition
  ```
  
* 不要同时使用 `unless` 和 `else`。把肯定的条件放到前面重写吧。
  
  ```
  # bad
  unless success?
      puts "failure"
  else
    puts "success"
  end
  
  # good
  if success?
    puts "success"
  else
    puts "failure"
  end
  ```
  
* 不要在 `if/unless/while` 的条件式两边加括号。

  ```
  # bad
  if （x > 10）
      # body omitted
  end
  
  # good
  if x > 10
      # body omitted
  end 
  ```
  
* 单行的块调用中请用 `{...}` 代替 `do…end`  。而多行的块调用则避免使用 `{…}`（而且多行的链式调用也往往很丑）。 对于 `流程控制` 和 `方法定义` 则往往使用 `do…end` （例如: 在 Rakefile 和某些 DSL 中）。 链式调用则要避免使用 `do…end`。

  ```
  names = ["Bozhidar", "Steve", "Sarah"]

  # good
  names.each { |name| puts name }

  # bad
  names.each do |name|
    puts name
  end

  # good
  names.select { |name| name.start_with?（"S"） }.map { |name| name.upcase }

  # bad
  names.select do |name|
    name.start_with?（"S"）
  end.map { |name| name.upcase }
  ```
  
某些人可能争论链式调用时，使用 `do…end` 看起来还可以，但是他们应该扪心自问——这样的代码真的可读吗?况且难道不能把块中的内容提取到精炼的方法里吗?

* 避免使用不必要的 `return` 。

  ```
  # bad
  def some_method（some_arr）
   return some_arr.size
  end

  # good
  def some_method（some_arr）
    some_arr.size
  end
  ```
  
* 当给方法参数赋予默认值的时候在 `=` 的两边添加空格 。

  ```
  # bad
  def some_method（arg1=:default, arg2=nil, arg3=[]）
    # do something...
  end

  # good
  def some_method（arg1 = :default, arg2 = nil, arg3 = []）
    # do something...
  end
  ```
  
  一些 Ruby 的书中可能建议第一种风格，但是第二种在实践中却更加卓越（而且还能稍稍增加可读性）。\

* 使用被括号包裹的 `=` （赋值语句）的返回值也是可以的。

  ```
  # bad
  if （v = array.grep（/foo/）） ...

  # good
  if v = array.grep（/foo/） ...

  # also good - has correct precedence.
  if （v = next_value） == "hello" ...
  ```

* 自如的使用 `||=` 来初始化变量吧。

  ```
  # set name to Bozhidar, only if it's nil or false
  name ||= "Bozhidar"
  ```

* 不要使用 `||=` 来初始化布尔变量。（想想看如果你变量当前的值为 `false` 时会发生什么。）

  ```
  # bad - would set enabled to true even if it was false
  enabled ||= true

  # good
  enabled = true if enabled.nil?
  ```

* 避免使用 Perl 风格的特殊变量（例如 `$0-9` 、 `$` 等等）。它们看起来非常神秘，除非用于单行脚本，否则不鼓励使用。使用完整形式的名称会更好一些,像是 `$PROGRAM_NAME` 。
* 永远不要在方法名和左括号之间放一个空格。

  ```
  # bad
  f （3 + 2） + 1

  # good
  f（3 + 2） + 1
  ```

* 如果方法的第一个参数以左括号起始，那么这个方法的调用需要使用括号。举例来说，要这样写 f（（3+2） + 1）。
* 使用 `_` 来命名用不到的块参数。

  ```
  # bad
  result = hash.map { |k, v| v + 1 }

  # good
  result = hash.map { |_, v| v + 1 }
  ```

## 命名
* 方法名使用蛇底式小写（ `snake_case` ）。
* 类名和模块名使用驼峰式大小写（ `CamelCase` ）。（首字母缩写词如 HTTP， RFC，XML 保持大写。）
* 其他常量使用尖叫蛇底式大写（ `SCREAMING_SNAKE_CASE` ）。
* 谓词方法（返回布尔值的方法）的名字应该以问好结尾。（即： `Array#empty?` ）。
* 潜在“危险”的方法（即会改变 `self` 或者参数的方法，如 `exit!` 等。）应该以叹号结尾。只有存在非危险方法时危险方法才存在。（[了解更多](http://dablog.rubypal.com/2007/8/15/bang-methods-or-danger-will-rubyist)）

## 类
* 由于类变量（ `@@` ）会在继承时产生不自然的行为所以避免使用它们。

  ```
  class Parent
    @@class_var = "parent"

    def self.print_class_var
      puts @@class_var
    end
  end

  class Child < Parent
    @@class_var = "child"
  end

  Parent.print_class_var # => will print "child"
  ```

  如同你所见，在类型层级中的所有类其实都共享同一个类变量。类实例变量通常比类变量更优先被使用。
* 使用 `def self.method` 来定义单例（ singleton ）方法。这使得方法更易重构。

  ```
  class TestClass
    # bad
    def TestClass.some_method
      # body omitted
    end

    # good
    def self.some_other_method
      # body omitted
    end
  end
  ```

* 避免使用 `class << self` 除非这是必须的， 比如单个读写器以及属性别名。

  ```
  class TestClass
    # bad
    class << self
      def first_method
        # body omitted
      end

      def second_method_etc
        # body omitted
      end
    end

    # good
    class << self
      attr_accessor :per_page
      alias_method :nwo, :find_by_name_with_owner
    end

    def self.first_method
      # body omitted
    end

    def self.second_method_etc
      # body omitted
    end
  end
  ```

## 异常处理
* 不要使用异常处理来控制流程。
* 避免直接处理 `Exception` 类。

## 集合

* 当你需要一个私服传数组的时候首选 `%w` 字符数组语法。

  ```
    # bad
  STATES = ["draft", "open", "closed"]

    # good
  STATES = %w(draft open closed)
  ```

* 处理多个无重复元素时使用 `Set` 代替 `Array` 。 `Set` 实现了无序、无重复值得集合。 `Set`的方法同数组类一样直观，和可以想哈希哪样快速查找元素。
* 尽量使用符号代替字符串作为哈希的键。

  ```
    # bad
  hash = { "one" => 1, "two" => 2, "three" => 3 }

    # good
  hash = { :one => 1, :two => 2, :three => 3 }
  ```

## 字符串
* 使用字符串插入（interpolation）替代字符串拼接（concatenation）。

  ```
    # bad
  email_with_name = user.name + " <" + user.email + ">"

    # good
  email_with_name = "#{user.name} <#{user.email}>"
  ```

* 尽量使用双括号字符串。这样无需改变定界符就可以照常使用插入值和转义字符，而且在字符串字面量中 `'` 比 `"` 更常见。

  ```
  # bad
  name = 'Bozhidar'

  # good
  name = "Bozhidar"
  ```

* 当你需要构造一个庞大的文本块时，避免使用 `String#+`。使用 `String#<<` 来替代。 `<<` 即时的修改被连结的字符串实例并且往往比创建了一堆新的字符串对象的 `String#+` 更快。

  ```
  # good and also fast
  html = ""
  html << "<h1>Page title</h1>"

  paragraphs.each do |paragraph|
    html << "<p>#{paragraph}</p>"
  end
  ```

## 正则表达式
* 避免使用 $1-9 ， 这将使其包含的内容变得难以追踪。具名分组是绝佳的替代方案。

  ```
  # bad
  /(regexp)/ =~ string
  ...
  process $1

  # good
  /(?<meaningful_var>regexp)/ =~ string
  ...
  process meaningful_var

  ```

* 小心的使用 `^` 与 `$`，他们匹配的是一行的开始与结尾， 而不是字符串的开始与结束。如果你想匹配整个字符串，使用 `\A` 和 `\Z`。

  ```
  string = "some injection\nusername"
  string[/^username$/]   # matches
  string[/\Ausername\Z/] # don't match
  ```

* 使用 `x` 来修饰复杂的正则。这将使他们更具可读性并且使你可以添加一些有用的注释。只是需要注意空白字符将被忽略。

  ```
  regexp = %r{
    start         # some text
    \s            # white space char
    (group)       # first group
    (?:alt1|alt2) # some alternation
    end
  }x
  ```
## 百分号字面量
* 自如的使用 `%w`。

  ```
  STATES = %w(draft open closed)
  ```

* 对于需要插入与嵌入双引号的单行字符串使用 %() 。而多行字符串，最好使用 heredocs 。

  ```
  # bad (no interpolation needed)
  %(<div class="text">Some text</div>)
  # should be "<div class="text">Some text</div>"

  # bad (no double-quotes)
  %(This is #{quality} style)
  # should be "This is #{quality} style"

  # bad (multiple lines)
  %(<div>\n<span class="big">#{exclamation}</span>\n</div>)
  # should be a heredoc.

   # good (requires interpolation, has quotes, single line)
  %(<tr><td class="name">#{name}</td>)
  ```

* `%r`专门用于要匹配多个 ‘/’ 字符的正则表达式时使用 。

  ```
  # bad
  %r(\s+)

  # still bad
  %r(^/(.*)$)
  # should be /^\/(.*)$/

  # good
  %r(^/blog/2011/(.*)$)
  ```

## 哈希
使用哈希火箭的语法作为哈希符号来替代 1.9 版本中引进的 JSON 风格。

  ```
  # bad
  user = {
    login: "Defunkt",
    name: "Chris Wanstrath"
  }

  # bad
  user = {
    login: "Defunkt",
    name: "Chris Wanstrath",
    "followers-count" => 52390235
  }

  # good
  user = {
    :login => "Defunkt",
    :name => "Chris Wanstrath",
    "followers-count" => 52390235
  }
  ```

## 总结及其它
Fllow your ❤
