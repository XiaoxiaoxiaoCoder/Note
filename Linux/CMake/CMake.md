# CMake
   
   
---

## 执行 
```
cmake path # 表示在指定目录path执行 cmake
```

### 基本编写逻辑
+ 设置 `CMake` 版本需求  `cmake_minimum_required`
+ 设置工程名  `project`
+ 生成可执行文件 `add_executable`
+ 生成静态库 `add_library`
+ 生成动态库 `add_library`
+ 链接可执行文件 `target_link_libraries`


## 语法

### if、条件表达式

+ 格式
```
if(expression1)
	COMMONAD1(ARGS ...)
	COMMONAD2(ARGS ...)	
	...
esleif(expression2)
	COMMONAD1(ARGS ...)
	COMMONAD2(ARGS ...)
	...
else()
	COMMONAD1(ARGS ...)
	COMMONAD2(ARGS ...)
	...
endif()
```

+ 关系操作符

|Name|Desc|
|:--|:--|
|NOT|非，NOT E1|
|AND|与，E1 AND E2|
|OR|或, E1 OR E2|
|EXIST|~E，存在name的文件或者目录(应该使用绝对路径)，真|
|COMMAND|~E，存在 command-name 命令、宏或函数且能够被调用，真|
|DEFINED|~E，变量被定义了,真|
|EQUAL|E1 ~ E2，变量值或者字符串匹配 regex 正则表达式|
|LESS|E1 ~ E2，变量值或者字符串匹配 regex 正则表达式|
|GREATER|E1 ~ E2，变量值或者字符串匹配 regex 正则表达式|
|STRLESS|E1 ~ E2，变量值或者字符串为有效的数字且满足小于（大于、等于）的条件|
|STRGREATER|E1 ~ E2，变量值或者字符串为有效的数字且满足小于（大于、等于）的条件|
|STREQUAL|E1 ~ E2，变量值或者字符串为有效的数字且满足小于（大于、等于）的条件|



 ### 变量、字符串

>`CMake`的基本数据类型是字符串(不区分大小写)，一组字符串在一起成为列表(list)。

+ 变量显示定义
```
set(VAR a) 		# 就是一个字符串
set(VAR a b c)  # 就是一个字符串list
```
+ [常用的部分内部变量](http://www.cnblogs.com/alexYuin/p/8874579.html)

+ 变量引用
```
使用${} 比如：${CMAKE_BINARY_DIR}
```

### include
> 用来载入 `CMakeList.txt` 文件，也用于载入预定义的cmake模块

```
include(cmake/OpenCVMinDepVersions.cmake)
```

### 循环
+ foreach
```
set(VAR a b c)
foreach(f ${VAR})
    message(${f})
endforeach()
```

+ while
```
set(VAR 5)
while(${VAR} GREATER 0)
    message(${VAR})
    math(EXPR VAR "${VAR} - 1")
endwhile()
```

### 函数
> 待后续补齐