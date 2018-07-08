# CMake
   
   
---

## 执行 
```
cmake path # 表示在指定目录path执行 cmake
```

### 基本编写逻辑
+ 设置 `cmake` 版本需求  `cmake_minimum_required`
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

 