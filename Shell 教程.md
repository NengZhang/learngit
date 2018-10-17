# Shell 教程 
Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。
Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。 
Ken Thompson 的 sh 是第一种 Unix Shell，Windows Explorer 是一个典型的图形界面 Shell。  

### Shell 脚本
Shell 脚本（shell script），是一种为 shell 编写的脚本程序。
业界所说的 shell 通常都是指 shell 脚本，但读者朋友要知道，shell 和 shell script 是两个不同的概念。
由于习惯的原因，简洁起见，本文出现的 "shell编程" 都是指 shell 脚本编程，不是指开发 shell 自身。 
### Shell 环境
Shell 编程跟 java、php 编程一样，只要有一个能编写代码的文本编辑器和一个能解释执行的脚本解释器就可以了。
Linux 的 Shell 种类众多，常见的有： 

- Bourne Shell（/usr/bin/sh或/bin/sh） 
- Bourne Again Shell（/bin/bash） 
- C Shell（/usr/bin/csh） 
- K Shell（/usr/bin/ksh） 
- Shell for Root（/sbin/sh） 
- …… 

本教程关注的是 Bash，也就是 Bourne Again Shell，由于易用和免费，Bash 在日常工作中被广泛使用。同时，Bash 也是大多数Linux 系统默认的 Shell。
在一般情况下，人们并不区分 Bourne Shell 和 Bourne Again Shell，所以，像 **#!/bin/sh**，它同样也可以改为 **#!/bin/bash**。
**#!** 告诉系统其后路径所指定的程序即是解释此脚本文件的 Shell 程序。

---

### <font color=#33cc77>第一个shell脚本</font>
打开文本编辑器(可以使用 vi/vim 命令来创建文件)，新建一个文件 test.sh，扩展名为 sh（sh代表shell），扩展名并不影响脚本执行，见名知意就好，如果你用 php 写 shell 脚本，扩展名就用 php 好了。
输入一些代码，第一行一般是这样：

```shell
#!/bin/bash
echo "Hello World !"
```
**#!** 是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种 Shell。 
echo 命令用于向窗口输出文本。 

#### <font color=#cc3333>运行 Shell 脚本有两种方法：</font>
##### **<font color=#333399>1、作为可执行程序</font>**  

将上面的代码保存为 test.sh，并 cd 到相应目录： 

```bash
chmod +x ./test.sh  #使脚本具有执行权限
./test.sh  #执行脚本
```
注意，一定要写成 **./test.sh**，而不是 **test.sh**，运行其它二进制的程序也一样，直接写 test.sh，linux 系统会去 PATH 里寻找有没有叫 test.sh 的，而只有 /bin, /sbin, /usr/bin，/usr/sbin 等在 PATH 里，你的当前目录通常不在 PATH 里，所以写成 test.sh 是会找不到命令的，要用 ./test.sh 告诉系统说，就在当前目录找。 

##### **<font color=#333399>2、作为解释器参数</font>** 

这种运行方式是，直接运行解释器，其参数就是 shell 脚本的文件名，如： 

```bash
/bin/sh test.sh
/bin/php test.php
```
这种方式运行的脚本，不需要在第一行指定解释器信息，写了也没用。

---

# Shell 变量
定义变量时，变量名不加美元符号（$，PHP语言中变量需要），如：  
```bash
your_name="runoob.com"
```
注意，变量名和等号之间不能有空格，这可能和你熟悉的所有编程语言都不一样。同时，变量名的命名须遵循如下规则：
- 命名只能使用英文字母，数字和下划线，首个字符不能以数字开头。
- 中间不能有空格，可以使用下划线（_）。 
- 不能使用标点符号。
- 不能使用bash里的关键字（可用help命令查看保留关键字）。

有效的 Shell 变量名示例如下：
```shell
RUNOOB
LD_LIBRARY_PATH
_var
var2
```
无效的变量命名：
```shell
?var=123
user*name=runoob
```
除了显式地直接赋值，还可以用语句给变量赋值，如：
```shell
for file in `ls /etc`
或
for file in $(ls /etc)
```
以上语句将 /etc 下目录的文件名循环出来。

---

#### <font color=#33cc77>使用变量</font>
使用一个定义过的变量，只要在变量名前面加美元符号即可，如：

```shell
your_name="qinjx"
echo $your_name
echo ${your_name}
```
变量名外面的花括号是可选的，加不加都行，加花括号是为了帮助解释器识别变量的边界，比如下面这种情况：
```shell
for skill in Ada Coffe Action Java; do
	echo "I am good at ${skill}Script"
done
```
如果不给skill变量加花括号，写成echo "I am good at $skillScript"，解释器就会把$skillScript当成一个变量（其值为空），代码执行结果就不是我们期望的样子了。
推荐给所有变量加上花括号，这是个好的编程习惯。
已定义的变量，可以被重新定义，如：

```shell
your_name="tom"
echo $your_name
your_name="alibaba"
echo $your_name
```
这样写是合法的，但注意，第二次赋值的时候不能写\$your_name="alibaba"，使用变量的时候才加美元符（$）。
#### <font color=#33cc77>只读变量</font>
使用 readonly 命令可以将变量定义为只读变量，只读变量的值不能被改变。
下面的例子尝试更改只读变量，结果报错：
```shell
#!/bin/bash
myUrl="http://www.google.com"
readonly myUrl
myUrl="http://www.runoob.com"
```
运行脚本，结果如下：
```shell
/bin/sh: NAME: This variable is read only.
```
#### <font color=#33cc77>删除变量</font>
使用 unset 命令可以删除变量。语法：
```shell
unset variable_name
```
变量被删除后不能再次使用。unset 命令不能删除只读变量。
实例
```shell
#!/bin/sh
myUrl="http://www.runoob.com"
unset myUrl
echo $myUrl
```
以上实例执行将没有任何输出。
#### <font color=#33cc77>变量类型</font>
运行shell时，会同时存在三种变量：
##### 局部变量
局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。
##### 环境变量 
所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。
##### shell变量
shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行

---

## Shell 字符串
字符串是shell编程中最常用最有用的数据类型（除了数字和字符串，也没啥其它类型好用了），字符串可以用单引号，也可以用双引号，也可以不用引号。单双引号的区别跟PHP类似。
# <font color=#33cc77>单引号</font>
```shell
str='this is a string'
```
单引号字符串的限制：
- 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
- 单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。
#### <font color=#33cc77>双引号</font>
```shell
your_name='runoob'
str="Hello, I know you are \"$your_name\"! \n"
echo $str
```
输出结果为：
```shell
Hello, I know you are "runoob"!
```
双引号的优点：
- 双引号里可以有变量
- 双引号里可以出现转义字符
#### <font color=#33cc77>拼接字符串</font>
```shell
your_name="runoob"
# 使用双引号拼接
greeting="hello, "$your_name" !"
greeting_1="hello, ${your_name} !"
echo $greeting  $greeting_1
# 使用单引号拼接
greeting_2='hello, '$your_name' !'
greeting_3='hello, ${your_name} !'
echo $greeting_2 $greeting_3
```
输出结果为：
```shell
hello, runoob ! hello, runoob !
hello, runoob ! hello, ${your_name} !
```
#### <font color=#33cc77>获取字符串长度</font>

```shell
string="abcd"
echo ${#string} #输出 4
```

#### <font color=#33cc77>提取子字符串</font>

以下实例从字符串第 2 个字符开始截取 4 个字符：

```shell
string="runoob is a great site"
echo ${string:1:4} # 输出 unoo
```

#### <font color=#33cc77>查找子字符串</font>

查找字符 **i** 或 **o** 的位置(哪个字母先出现就计算哪个)：

```shell
string="runoob is a greate site"
echo `expr index "$string" io`  # 输出 4`
```

**注意：** 以上脚本中 ` 是反引号，而不是单引号 '，不要看错了哦。

---

## Shell 数组

bash支持一维数组（不支持多维数组），并且没有限定数组的大小。

类似于 C 语言，数组元素的下标由 0 开始编号。获取数组中的元素要利用下标，下标可以是整数或算术表达式，其值应大于或等于 0。

#### 定义数组

在 Shell 中，用括号来表示数组，数组元素用"空格"符号分割开。定义数组的一般形式为：

```shell
数组名=(值1 值2 ... 值n)
```

例如：

```shell
array_name=(value0 value1 value2 value3)
```

或者

```shell
array_name=(
value0
value1
value2
value3
)
```

还可以单独定义数组的各个分量：

```shell
array_name[0]=value0
array_name[1]=value1
array_name[2]=valuen
```

可以不使用连续的下标，而且下标的范围没有限制。

#### 读取数组

读取数组元素值的一般格式是：

```shell
${数组名[下标]}
```

例如：

```shell
valuen=${array_name[n]}
```

使用 @ 符号可以获取数组中的所有元素，例如：

```shell
echo ${array_name[@]}
```

#### 获取数组的长度

获取数组长度的方法与获取字符串长度的方法相同，例如：

```shell
# 取得数组元素的个数
length=${#array_name[@]}
# 或者
length=${#array_name[*]}
# 取得数组单个元素的长度
length=${#array_name[n]}
```

---

### Shell 注释

以 # 开头的行就是注释，会被解释器忽略。

通过每一行加一个 **#** 号设置多行注释，像这样：

```shell
#--------------------------------------------
# 这是一个注释
# author：菜鸟教程
# site：www.runoob.com
# slogan：学的不仅是技术，更是梦想！
#--------------------------------------------
##### 用户配置区 开始 #####
#
#
# 这里可以添加脚本描述信息
# 
#
##### 用户配置区 结束  #####
```

如果在开发过程中，遇到大段的代码需要临时注释起来，过一会儿又取消注释，怎么办呢？

每一行加个#符号太费力了，可以把这一段要注释的代码用一对花括号括起来，定义成一个函数，没有地方调用这个函数，这块代码就不会执行，达到了和注释一样的效果。

#### 多行注释

多行注释还可以使用以下格式：

```shell
:<<EOF
注释内容...
注释内容...
注释内容...
EOF
```

EOF 也可以使用其他符号:

```shell
:<<'
注释内容...
注释内容...
注释内容...
'

:<<!
注释内容...
注释内容...
注释内容...
!
```

---

# Shell 传递参数

我们可以在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：**$n**。**n** 代表一个数字，1 为执行脚本的第一个参数，2 为执行脚本的第二个参数，以此类推……

**实例**

以下实例我们向脚本传递三个参数，并分别输出，其中 **$0** 为执行的文件名：

```shell
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

echo "Shell 传递参数实例！";
echo "执行的文件名：$0";
echo "第一个参数为：$1";
echo "第二个参数为：$2";
echo "第三个参数为：$3";
```

为脚本设置可执行权限，并执行脚本，输出结果如下所示：

```shell
$ chmod +x test.sh
$ ./test.sh 1 2 3
Shell 传递参数实例！
执行的文件名：./test.sh
第一个参数为：1
第二个参数为：2
第三个参数为：3
```

另外，还有几个特殊字符用来处理参数：

| 参数处理 | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| $#       | 传递到脚本的参数个数                                         |
| $*       | 以一个单字符串显示所有向脚本传递的参数。<br/>如 "\$*" 用「"」(双引号) 括起来的情况、以 "\$1 \$2 ... \$n" 的形式输出所有参数。 |
| $$       | 脚本运行的当前进程ID号                                       |
| $!       | 后台运行的最后一个进程ID号                                   |
| $@       | 与 \$* 相同，但是使用时加引号，并在引号中返回每个参数。<br/>如 "\$@" 用「"」(双引号)括起来的情况、以 "\$1" "\$2" ... "\$n" 的形式输出所有参数。 |
| $-       | 显示Shell使用的当前选项，与<font color=#9933cc>set命令</font>功能相同。 |
| $?       | 显示最后命令的推出状态。0表示没有错误，其他任何值表明有错误  |

```shell
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

echo "Shell 传递参数实例！";
echo "第一个参数为：$1";

echo "参数个数为：$#";
echo "传递的参数作为一个字符串显示：$*";
```

执行脚本，输出结果如下所示：

```bash
$ chomd +x test.sh
$ ./test.sh 1 2 3
Shell 传递参数实例！
第一个参数为：1
参数个数为：3
传递的参数作为一个字符串显示：1 2 3
```

\$* 与 \$@ 区别：

- 相同点：都是引用所有参数
- 不同点：只有在双引号中体现出来。假设在脚本运行时写了三个参数 1、2、3，则 " \$* " 等价于 "1 2 3" (传递了一个参数)，而 " \$@ " 等价于 "1"  "2"  "3" (传递了三个参数)。

```shell
#!bin/bash
# author:菜鸟教程
# url:www.runoob.com

echo "-- \$* 演示 ---"
# echo "-- $* 演示 ---"
for i in "$*"; do
	echo $i
done

echo "-- \$@ 演示 ---"
for i in "$@"; do
	echo $i
done
```

执行脚本，输出结果如下所示：

```bash
$ chmod +x test.sh
$ ./test.sh 1 2 3
-- $* 演示 ---
1 2 3
-- $@ 演示 ---
1
2
3
```























