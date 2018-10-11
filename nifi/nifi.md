# 表达式语言概述

Apache NiFi中的所有数据都由称为FlowFile的东西抽象表示。FlowFile主要由两个部分组成：content 和attributes。

FlowFile的content 部分表示要操作的数据。例如，如果使用GetFile Processor从本地文件系统中获取文件，则文件的内容将成为FlowFile的content 。

FlowFile的attributes部分表示有关数据本身的信息,或者是元数据。属性是一些键值对，表示对数据的已知信息以及对能够适当路由和处理数据的有用信息。例如从本地文件系统中拾取的文件的示例，FlowFile将具有名为filename的属性，该属性表示文件系统上的文件的名称。此外，FlowFile将具有一个path属性，该属性表示该文件所在的文件系统上的目录.FlowFile还将具有名为uuid的属性，该属性是此FlowFile的唯一标识符。有关核心属性的完整列表，请查看[Developer's Guide](https://nifi.apache.org/docs/nifi-docs/html/developer-guide.html#flowfile)的FlowFile部分。

但是，如果用户无法使用这些属性的话，则将这些属性放在FlowFile上并没有什么卵用。NiFi的表达式语言恰恰提供了引用这些属和将它们与其他值进行比较,以及操纵它们的值的功能。

# 表达式语言的结构

NiFi表达式语言始终以${expr}表示。在开始和结束分隔符之间是表达式本身的文本。在最基本的形式中，Expression只能包含一个属性名称。例如，`${filename}`将返回filename属性的值。

在一个稍微复杂点的例子中，我们可以返回对此值的操作。例如，我们可以通过调用`toUpper`函数返回文件名的全大写版本,例如,${filename:toUpper()}。在这种情况下，我们引用了filename这个属性,并使用toUpper函数操纵这个值。

一个函数调用由5部分组成,首先,是函数调用的分割符冒号 :   ,第二 个是函数名,这个例子中是toUpper,然后是左括号,再然后是函数参数,是否需要参数依赖于调用的函数,在这个例子中,toUpper函数没有参数,因此就省略了.最后，右括号,表示函数调用的结束。表达式语言支持许多不同的函数，以实现许多不同的功能。一些函数提供String（文本）操作，例如toUpper函数. 还有一些函数提供比较功能,例如equal,match函数。还有用于操纵日期和时间以及执行数学运算的函数。下面在[函数](https://nifi.apache.org/docs/nifi-docs/html/expression-language-guide.html#functions)部分中描述了函数的作用，它所需的参数以及它返回值类型。

当我们对属性执行函数调用时，就像上边的例子，我们将该属性称为要操作的函数的 *subject*，因为该属性是函数运行的实体。然后我们可以将多个函数调用串联在一起，其中第一个函数的返回值将成为第二个函数的subject，其返回值成为第三个函数的subject，依此类推。延用我们的示例，我们可以使用表达式将多个函数链接在一起,例如`${filename:toUpper():equals('HELLO.TXT')}`,可以链接在一起的函数的数量没有限制的。



可以使用表达式语言引用任何FlowFile属性。但是，如果属性名称包含特殊字符,那么特殊字符必须转移才能使用,下边是所有的特殊字符:

- $ (dollar sign)
- | (pipe)
- { (open brace)
- } (close brace)
- ( (open parenthesis)
- ) (close parenthesis)
- [ (open bracket)
- ] (close bracket)
- , (comma)
- : (colon)
- ; (semicolon)
- / (forward slash)
- \* (asterisk)
- ' (single quote)
- (space)
- \t (tab)
- \r (carriage return)
- \n (new-line)

此外，如果数字是属性名称的第一个字符，则该数字被视为特殊字符。

如果属性中存在任何这些特殊字符，则使用单引号或双引号引用。表达式语言允许单引号和双引号可互换使用。例如，以下内容可用于转义名为my attribute'的属性：$ {“my attribute”}或$ {'my attribute'},单双引号都可以的.

在这个示例中，返回的值是“my attribute属性的值（如果存在的话）。如果该属性不存在，则表达式语言将在系统环境变量中查找名为“my attribute”的属性。如果找不到，它将在JVM系统中查找名为“my attribute”的系统属性。最后，如果这些都不存在，则表达式语言将返回null值。

还有一些函数是没有subject的,只需简单在表达式开头使用即可,例如 $ {hostname()}。这些功能也可以一起使用。例如，$ {hostname():toUpper()}。如果试图使用subject去执行这些函数会导致错误,在下面的“Function”部分中，这些函数将在其描述中清楚地表明它们不需要subject。

经常的，我们需要将两个不同属性的值相互比较。我们可以通过使用嵌入式表达式来实现这一目标。例如，我们可以这样做,去检查filename属性是否与uuid属性值相同：$ {filename：equals（$ {uuid}）}。另请注意，我们在equals方法的左括号和嵌入式表达式之间有一个空格。这不是必需的，并且不会影响表达式的执行。相反，它旨在使表达更容易阅读。分隔符之间的空格会被表达式语言忽略。因此，我们可以使用 $ {filename:equals($ {uuid})}或$ {filename:equals($ {uuid})}，两个表达式的含义相同。但是，我们不能使用$ {file name:equals( {uuid})}，filename被拆开了,这会导致file和name被解释为不同的标记，而不是单个标记filename

。

# 安装nifi

修改配置文件conf/nifi.properties 

```
nifi.web.http.host=192.168.140.130
nifi.web.http.port=8080
```

启动/停止/状态

```
bin/nifi.sh start

bin/nifi.sh stop

bin/nifi.sh status
```

**UI**界面

http://192.168.140.130:8080

# NiFi组件

- FlowFile 
  `一个FlowFile代表每个被系统处理的数据对象`，一个FlowFile由两部分组成：属性（FlowFile Attributes）和内容（FlowFile Content）。内容是数据本身，属性是与数据相关的key-value的键值对，用于描述数据。所有flowFile都有以下标准属性：uuid、filename、path
- FlowFile Processor 
  Processor是NiFi的组件，`可以用来创建、发送、接受、转换、路由、割裂、合并、处理FlowFiles`。在用户建立数据流时，Processor是最重要的组成部分
- Connection 
  `提供Processors之间的连接，用来定义Processors之间的执行关系`，并允许不同Processors之间以不同的速度进行交互
- Flow Controller 
  其负责维护Processors之间的关联信息，并且管理所有进程对于线程的使用、分配
- Process Group 
  一个特定集合的Processors与它们之间的连接关系形成一个Process Group，其定义了从接受端口接受数据到通过发送端口发送数据之间，整个数据流的处理过程。并可以通过简单组合其它的部件来创建新的部件

远程debug

代码重新调试一次 