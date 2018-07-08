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

 