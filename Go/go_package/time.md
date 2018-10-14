# Time 包使用
---
### 简介
`time` 顾名思义，该包提供了时间相关的操作`function`,使用需要 `import "time"`

---

### 数据类型
主要数据类型有以下几种
```
type Time struct{		// 时间主要结构
	... // 内容省略
}

// A Month specifies a month of the year (January = 1, ...).
type Month int 		  // 月份别名,主要为了实现月份的输出 实现了 Stringer 接口

// A Weekday specifies a day of the week (Sunday = 0, ...).
type WeekDay int 	   // 一周中的第几天， 实现了 Stringer 接口

type Duration int64     // 时间精度，最小精度为纳秒
```
`Month`、`WeekDay`没有相关的API,只为了输出string类型实现了 `Stringer` 接口,主要`time`、`Time`、`Duration`

---

### API

time 包 API
```
func Now() Time 	// 返回当前时间Time

func Unix(sec int64, nsec int64) Time // 返回sec(时间戳)、nsec(纳秒范围)的时间描述Time 

func Since(t Time) Duration // 返回指定时间距离当前时间已经过去的时间

func Until(t Time) Duration // 返回指定时间局当前时间还有的时间

func Date(year int, month Month, day, hour, min, sec, nsec int, loc *Location) Time // 指定日期、时间、时区，返回定时间Time

例子：
now := time.Now() // 获取当前时间
dateTime := time.Date(2018, 10, 10, 0, 0, 0, 0, time.Local) // 获取2018年10月10号0点0分0秒的时间Time
```
Time API
```
func (t Time) Unix() int64 // 返回时间戳,精度秒

func (t Time) UnixNano() int64 // 返回时间戳,精度纳秒

func (t Time) UTC() Time // 返回 UTC 时区的时间Time

func (t Time) After(u Time) bool // 检查时间是否在时间 u 之后

func (t Time) Before(u Time) bool // 检查时间是否在时间 u 之前

func (t Time) Equal(u Time) bool // 检查时间是否与 u 相同

func (t Time) IsZero() bool // 检查是否为零时，即格林威治时间0点

func (t Time) Date() (year int, month Month, day int)  // 返回年月日

func (t Time) Year() int  // 返回年

func (t Time) Month() Month // 返回月

func (t Time) Day() int // 返回日

func (t Time) Weekday() Weekday // 返回一周中的第几天

func (t Time) ISOWeek() (year, week int) // 返回年和当前一年中第几周

func (t Time) Clock() (hour, min, sec int) // 返回时分秒

func (t Time) Hour() int  // 返回小时

func (t Time) Minute() int  // 返回分钟

func (t Time) Second() int  // 返回秒

func (t Time) Nanosecond() int // 返回纳秒部分值

func (t Time) YearDay() int  // 返回一年中的第几天

func (t Time) Add(d Duration) Time // 返回 time +d 的时间

func (t Time) Sub(u Time) Duration // 返回 time -d 的时间

func (t Time) AddDate(years int, months int, days int) Time // 返回加上指定年、月、日的时间

func (t Time) Local() Time // 放回当前时区的time

func (t Time) In(loc *Location) Time // 设置时区

func (t Time) Location() *Location // 返回时区信息

例子：
// 获取当前0点0分0秒的时间
now := time.Now()
hour, minute, second := now.Clock()
dayStartTimeStamp := now.Add(time.Duration(-1*(3600*hour+60*minute+second)) * time.Second)
addDate := now.AddDate(-1, 1, 1)
fmt.Println("Sub:", now.Sub(addDate))
```
Duration API
```
func (d Duration) Nanoseconds() int64 // 返回纳秒级精度数据

func (d Duration) Seconds() float64 // 返回秒级精度数据

func (d Duration) Minutes() float64 // 返回分钟级精度数据

func (d Duration) Hours() float64 // 返回小时级精度数据

```

---

### Exmaple
```
package main

import (
	"fmt"
	"time"
)

func main() {

	now := time.Now()
	fmt.Println("Now Time:", now) // 系统设置的时区时间
	fmt.Println("Now Timestamp:", now.Unix())
	fmt.Println("Now Timestamp Nano:", now.UnixNano())
	fmt.Println("Now UTC:", now.UTC()) // UTC 时区

	year, week := now.ISOWeek()
	fmt.Printf("year:%v week:%v\n", year, week)

	hour, minute, second := now.Clock()
	fmt.Printf("%v:%v:%v\n", hour, minute, second)
	fmt.Println("Houre:", now.Hour())
	fmt.Println("Minute", now.Minute())
	fmt.Println("Second", now.Second())
	fmt.Println("NanoSecond:", now.Nanosecond())

	// 获取当天0点的时间戳
	dayStartTimeStamp := now.Add(time.Duration(-1*(3600*hour+60*minute+second)) * time.Second)
	fmt.Println(dayStartTimeStamp)

	addDate := now.AddDate(-1, 1, 1)
	fmt.Println("AddDate:", addDate)

	fmt.Println("Sub:", now.Sub(addDate).Hours())

	// 获取指定日期的时间戳
	curTime := time.Date(2018, 10, 10, 0, 0, 0, 0, time.Local)
	fmt.Println(curTime)
	// 0点时间
	zeroTime := time.Date(1, 1, 1, 0, 0, 0, 0, time.UTC)
	fmt.Println("zero:", zeroTime.IsZero())

	fmt.Println(int(time.Since(curTime)) / 1e9)
	fmt.Println(time.Since(curTime).Seconds())
	fmt.Println(int(time.Until(curTime)) / 1e9)
}

// 输出--时间相关，每次输出是时间点都不一样
Now Time: 2018-10-14 16:21:11.575369147 +0800 CST m=+0.002621862
Now Timestamp: 1539505271
Now Timestamp Nano: 1539505271575369147
Now UTC: 2018-10-14 08:21:11.575369147 +0000 UTC
year:2018 week:41
16:21:11
Houre: 16
Minute 21
Second 11
NanoSecond: 575369147
2018-10-14 00:00:00.575369147 +0800 CST m=-58870.997378138
AddDate: 2017-11-15 16:21:11.575369147 +0800 CST
Sub: 7992
2018-10-10 00:00:00 +0800 CST
zero: true
404471
404471.576116895
-404471

```

