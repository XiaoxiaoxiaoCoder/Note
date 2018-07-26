# Shell 
---



## Shell 变量

### 使用变量

> 定义变量的时候,变量和等号之间不能有空格,使用变量的时候加上美元符号，比如${var}


```
var=123
echo ${var}
str_var="123"
echo ${str_var}
```

### 只读变量
> 只读变量,只能读取,不能修改,不能删除

```
read_only_var="hello"
readonly read_only_var
echo ${read_only_var}
```

### 删除变量
> 不能删除只读变量

```
var="hello"
unset var
echo ${var}
```


---


## 字符串
>shell字符串可以使用单引号，也可以用双引号。

### 单引号
```
str='this is a string'
```
单引号字符串的限制：
+ 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的
+ 单引号字串中不能出现单引号（对单引号使用转义符后也不行）

### 双引号
```
name="Kevin"
str="Hello, I know yor are \"${name}\"!\n"
```
双引号有点：
+ 双引号里可以有变量
+ 双引号里可以出现转义字符

### 拼接字符串
>直接拼接

```
name="Kevin"
greeting_1="hello, "${name}"!"
greeting_2="hello, ${name}!"
echo ${greeting_1}
echo ${greeting_2}
```

### 获取字符串长度
```
name="Kevin"
echo ${#name}
```

### 提取子字符串
```
name="Kevin"
echo ${name:1:4}
```

### 查找子字符串
```
string="runoob is a great company"
echo `expr index "$string" is`
```

---

## 数组
>bash 支持一维数组，不支持多维数组

### 定义数组
>在shell中，用括号来表示数组，数组元素用`空格`符号分割开。

```
数组名=(值1 值2 值3 ... 值n)

array_name=(value0 value1 value2 value3)
array_name=(
value0
value1
value2
value3
)
```

### 读取数组
> 读取数组元素的一般格式是: `${数组名[下标]}`

```
value=${array_name[n]}
```
使用`@`符号可以获取数组中的所有元素
```
echo ${array_name[@]}
```

### 获取数组长度
>获取数组长度的方法与获取字符串长度的方法相同

```
# 获取数组元素个数
length=${#array_name[@]}
# 或者
length=${#array_name[*]}
# 取得单个元素的长度
length=${#array_name[n]}
```
---
## shell 传递参数
>执行shell脚本时，向脚本传递参数，脚本内获取参数的格式为：$n，n代表一个数字，1为执行脚本的第一个参数，2为执行脚本的第二个参数，以此内推...

实例：
```
#!/bin/bash

echo "shell 传递参数实例"
echo "执行文件名: $0"
echo "第一个参数: $1"
echo "第二个参数: $2"

echo "参数个数为: $#"
echo "传递的参数作为一个字符串显示: $*"

for i in "$*"
do
    echo ${i}
done

for i in "$@"
do
    echo ${i}
done
```

输出为
```
shell 传递参数实例
执行文件名: args.sh
第一个参数: 3
第二个参数: 4
参数个数为: 2
传递的参数作为一个字符串显示: 3 4
3 4
3
4
```

参数说明

| 参数处理 | 说明 |
|---|:--|
|$#|传递到脚本的参数个数|
|$*|以一个单字符串显示所有向脚本传递的参数|
|$$|脚本运行的当前进程ID|
|$!|后台运行的最后一个进程ID|
|$@|与`$*`相同,但是使用时加引号,并在引号中返回每个参数|
|$-|显示shell使用的当前选项,与`set命令`相同|
|$?|显示最后命令的退出状态。0表示没有错误,其他任何值表明有错误|

---

## Shell 运算符
>shell和其他变成语言,支持多种运算符

+ 算数运算符
+ 关系运算符
+ 布尔运算符
+ 字符串运算符
+ 文件测试运算符

```
#!/bin/bash

val=`expr 2 + 2`
echo "两数之和为 : $val"
```
输出为：
```
两数之和为 : 4
```
两点注意：
+ 表达式和运算符之间还有空格
+ 完整的表达式要被` `` `包含

### 算数运算符
下表列出了常用的关系运算符，假定变量 a 为 10，变量 b 为 20：

| 运算符 | 说明 | 举例 |
|-----|:--|:--|
|+|加法|\`expr ${a} + ${b}\` 结果为30|
|-|减法|\`expr ${a} - ${b}\` 结果为-10|
|*|乘法|\`expr ${a} \\* ${b}\` 结果为200|
|/|除法|\`expr ${b} / ${a}\` 结果为2|
|%|取余|\`expr ${b} % ${a}\` 结果为0|
|=|赋值|a=${b} 将把变量b赋值给a|
|==|相等,用于比较两个数字,相同则返回true|[ ${a}==${b} ] 返回false|
|!=|不相等,用于比较两个数字,不相同则返回true|[ ${a}!=${b} ] 返回true|

>注意：乘号(*)前面必须加反斜杠(\)才能实现乘法运算

### 关系运算符
关系运算符只支持数字,不支持字符串,除非字符串的值是数字。
下表列出了常用的关系运算符，假定变量 a 为 10，变量 b 为 20：

| 运算符 | 说明 | 举例 |
|-----|:--|:--|
|-eq|检测两个数是否相等,相等则返回true|[ ${a} -eq ${b} ]返回false|
|-nq|检测两个数是否不相等,不相等则返回true|[ ${a} -eq ${b} ]返回true|
|-gt|检测左边的数是否大于右边的,如果是则返回true|[ ${a} -gt ${b} ]返回false|
|-lt|检测左边的数是否小于右边的,如果是则返回true|[ ${a} -lt ${b} ]返回true|
|-ge|检测左边的数是否大于等于右边的,如果是则返回true|[ ${a} -ge ${b} ]返回false|
|-le|检测左边的数是否小于等于右边的,如果是则返回true|[ ${a} -le ${b} ]返回true|


### 布尔运算符
下表列出了常用的关系运算符，假定变量 a 为 10，变量 b 为 20：

| 运算符 | 说明 | 举例 |
|-----|:--|:--|
|!|非运算,表达式为true,则返回false,否则返回true| [!false] 返回true|
|-o|或运算,有一个表达式为true,则返回true| [ ${a} -lt 20 -o ${b} -gt 100] 返回true|
|-a|与运算,两个表达式都为true,则返回true| [ ${a} -lt 20 -a ${b} -gt 100] 返回false|

### 逻辑运算符
以下介绍 Shell 的逻辑运算符，假定变量 a 为 10，变量 b 为 20:

| 运算符 | 说明 | 举例 |
|-----|:--|:--|
|&&|逻辑AND|[[ ${a} -lt 100 && ${b} -gt 100 ]] 返回 false|
| &#124;  &#124;  |逻辑OR|[[ ${a} -lt 100 &#124; &#124; ${b} -gt 100 ]] 返回 true|


### 字符串运算符
下表列出了常用的字符串运算符，假定变量 a 为 "abc"，变量 b 为 "efg"：

| 运算符 | 说明 | 举例 |
|-----|:--|:--|
|=|检测两个字符串是否相等,相等返回true|[ ${a} = ${b} ]返回false|
|!=|检测两个字符串是否相等,不相等返回true|[ ${a} != ${b} ]返回true|
|-z|检测字符串长度是否为0,为0返回true|[ -z ${a} ]返回false|
|-n|检测字符串长度是否为0,不为0返回true|[ -n ${a} ]返回true|
|str|检测字符串长度是否为空,不为空返回true|[ ${a} ]返回true|


### 文件运算符
文件测试运算符用于检测 Unix 文件的各种属性。

| 运算符 | 说明 | 举例 |
|-----|:--|:--|
|-b file|检测文件是否是块设备文件,如果是,则返回 true|	[ -b ${file} ] 返回 false|
|-c file|检测文件是否是字符设备文件,如果是,则返回 true|[ -c ${file} ] 返回 false|
|-d file|检测文件是否是目录,如果是,则返回 true|	[ -d ${file} ] 返回 false|
|-f file|检测文件是否是普通文件(既不是目录,也不是设备文件),如果是,则返回 true|[ -f ${file} ] 返回 true|
|-g file|检测文件是否设置了SGID位,如果是,则返回 true|	[ -g ${file} ] 返回 false|
|-k file|检测文件是否设置了粘着位(Sticky Bit),如果是,则返回 true|	[ -k ${file} ] 返回 false|
|-p file|检测文件是否是有名管道,如果是,则返回 true|	[ -p ${file} ] 返回 false|
|-u file|检测文件是否设置了SUID位,如果是,则返回 true|	[ -u ${file} ] 返回 false|
|-r file|检测文件是否可读,如果是,则返回 true|	[ -r ${file} ] 返回 true|
|-w file|检测文件是否可写,如果是,则返回 true|	[ -w ${file} ] 返回 true|
|-x file|检测文件是否可执行,如果是,则返回 true|	[ -x ${file} ] 返回 true|
|-s file|检测文件是否为空(文件大小是否大于0),不为空则返回 true|	[ -s ${file} ] 返回 true|
|-e file|检测文件(包括目录)是否存在,如果是,则返回 true|[ -e ${file} ] 返回 true|

---

## test 命令
Shell中的 test 命令用于检查某个条件是否成立，它可以进行数值、字符和文件三个方面的测试。

>test 命令主要使用上面的运算符进行测试

```
num1=100
num2=100

if test ${num1} -eq ${num2}
then
    echo '两个数相等'
else
    echo '两个数不相等'
fi
```

---

## 流程控制

### if 语句
```
if condition
then
    command1 
    command2
    ...
    commandN 
fi
```

### if else 语句
```
if condition
then
    command1 
    command2
    ...
    commandN
else
    command
fi
```

### if else-if else 语句
```
if condition1
then
    command1
elif condition2 
then 
    command2
else
    commandN
fi
```

exmaple:
```
a=10
b=20
if [ $a == $b ]
then
   echo "a 等于 b"
elif [ $a -gt $b ]
then
   echo "a 大于 b"
elif [ $a -lt $b ]
then
   echo "a 小于 b"
else
   echo "没有符合的条件"
fi

输出：
a 小于 b
```

### for 循环
```
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```

### while 语句
```
while condition
do
    command
done
```
example:
```
for loop in 1 2 3 4 5
do
    echo "The value is: $loop"
done

输出：
The value is: 1
The value is: 2
The value is: 3
The value is: 4
The value is: 5
```

example:
```
#!/bin/bash
int=1
while(( $int<=5 ))
do
    echo $int
    let "int++"
done

输出：
1
2
3
4
5
```

### until 循环
```
until condition
do
    command
done
```

example:
```
#!/bin/bash

a=0

until [ ! $a -lt 10 ]
do
   echo $a
   a=`expr $a + 1`
done

输出：
0
1
2
3
4
5
6
7
8
9
```

### case
```
case 值 in
模式1)
    command1
    command2
    ...
    commandN
    ;;
模式2）
    command1
    command2
    ...
    commandN
    ;;
esac
```

example:
```
echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case $aNum in
    1)  echo '你选择了 1'
    ;;
    2)  echo '你选择了 2'
    ;;
    3)  echo '你选择了 3'
    ;;
    4)  echo '你选择了 4'
    ;;
    *)  echo '你没有输入 1 到 4 之间的数字'
    ;;
esac

输出：
输入 1 到 4 之间的数字:
你输入的数字为:
3
你选择了 3
```

### break 
break命令允许跳出所有循环（终止执行后面的所有循环）。
```
#!/bin/bash
while :
do
    echo -n "输入 1 到 5 之间的数字:"
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的! 游戏结束"
            break
        ;;
    esac
done

输出:
输入 1 到 5 之间的数字:3
你输入的数字为 3!
输入 1 到 5 之间的数字:7
你输入的数字不是 1 到 5 之间的! 游戏结束
```

### continue 
continue命令与break命令类似，只有一点差别，它不会跳出所有循环，仅仅跳出当前循环

```
#!/bin/bash
while :
do
    echo -n "输入 1 到 5 之间的数字: "
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的!"
            continue
            echo "游戏结束"
        ;;
    esac
done
```
---

## 函数

shell 中函数定义格式如下:
```
[function] funname[()]
{
    action;

    [return int]
}
```
说明：
+ 可以带function fun() 定义,也可以直接fun()定义,不带任何参数
+ 参数返回,可以显示加:return 返回,如果不加,将以最后一条命令运行结果,作为返回值。

example:
```
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

demoFun(){
    echo "这是我的第一个 shell 函数!"
}
echo "-----函数开始执行-----"
demoFun
echo "-----函数执行完毕-----"

输出:
-----函数开始执行-----
这是我的第一个 shell 函数!
-----函数执行完毕-----
```

下面是一个带return的函数
```
funWithReturn(){
    echo "这个函数会对输入的两个数字进行相加运算..."
    echo "输入第一个数字: "
    read aNum
    echo "输入第二个数字: "
    read anotherNum
    echo "两个数字分别为 $aNum 和 $anotherNum !"
    return $(($aNum+$anotherNum))
}
funWithReturn
echo "输入的两个数字之和为 $? !"

输出：
这个函数会对输入的两个数字进行相加运算...
输入第一个数字: 
1
输入第二个数字: 
2
两个数字分别为 1 和 2 !
输入的两个数字之和为 3 !
```

### 函数参数

在Shell中，调用函数时可以向其传递参数。在函数体内部，通过 $n 的形式来获取参数的值，例如，$1表示第一个参数，$2表示第二个参数...

```
funWithParam(){
    echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第十个参数为 $10 !"
    echo "第十个参数为 ${10} !"
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个!"
    echo "作为一个字符串输出所有参数 $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73

输出结果:
第一个参数为 1 !
第二个参数为 2 !
第十个参数为 10 !
第十个参数为 34 !
第十一个参数为 73 !
参数总数有 11 个!
作为一个字符串输出所有参数 1 2 3 4 5 6 7 8 9 34 73 !
```
>注意：$10 不能获取第十个参数，获取第十个参数需要${10}。当n>=10时，需要使用${n}来获取参数。


