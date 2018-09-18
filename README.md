# cron

基于 github.com/robfig/cron 的一个改进版本, 在原有基础上支持带 jobname 的定时器，

方便删除定时器中的任务。

带 jobname 的定时器如下所示

###
测试例子如下:
```
package main

import (
    "fmt"
    "time"

    "github.com/zommage/cron"
)

const (
    Time_Format = "2006-01-02 15:04:05"
)

func print1() {
    fmt.Println("111 now time: ", time.Now().Format(Time_Format))
}

func printa() {
    fmt.Println("aaa now time: ", time.Now().Format(Time_Format))
}

func printb() {
    fmt.Println("bbb now time: ", time.Now().Format(Time_Format))
}

func printc() {
    fmt.Println("ccc now time: ", time.Now().Format(Time_Format))
}

func main() {
    fmt.Println("init cron")
    cronRes := cron.New()
	
    //不带 jobname 的定时器 
    cronRes.AddFunc("*/2 * * * * ?", print1)

    //带 jobname 的定时器, 分别创建名字为 aaa 和 bbb 的定时任务
    cronRes.AddFuncWithName("*/2 * * * * ?", printa, "aaa")
    cronRes.AddFuncWithName("*/2 * * * * ?", printb, "bbb")

    cronRes.Start()
    defer cronRes.Stop()

    time.Sleep(4 * time.Second)
    cronRes.AddFuncWithName("*/2 * * * * ?", printc, "ccc")

    //删除名字为 bbb的 job
    err := cronRes.RemoveJob("bbb")
    if err != nil {
        fmt.Println("remove job is err: ", err)
        return
	}

    time.Sleep(4 * time.Second)
	
    return
}
```
###
