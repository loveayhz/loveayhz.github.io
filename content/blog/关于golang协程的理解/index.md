---
title: 关于goolang协程的理解
date: "2024-07-25T18:00:32.169Z"
description: 关于golang协程的理解.
---

## 关于goolang协程的理解
参考: [go channel 万字详解](https://zhuanlan.zhihu.com/p/643013131)

手动写一下执行代码看看，加深理解。

```
func TestSingleDeirection(t *testing.T) {
	in := make(chan int, 10)
	doneR := make(chan bool)
	go func() {
		for e := range in {
			fmt.Println(e)
		}
		doneR <- true
	}()

	go func() {
		for i := 0; i < 10; i++ {
			in <- (i * i)
		}
		close(in)
	}()
	<-doneR
	fmt.Print("执行完毕")
}
```

```
func TestStopRead(t *testing.T) {
	ch := make(chan int, 10)
	go func() {
		for i := 0; i < 10; i++ {
			ch <- i
		}
		close(ch)
	}()
	for {
                //关闭通道作为信号，ok=false
		v, ok := <-ch
		if !ok {
			break
		}
		fmt.Println(v)
	}
	fmt.Println("结束")
}