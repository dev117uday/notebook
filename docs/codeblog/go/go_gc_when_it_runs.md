

# Golang 101 : Looking at when Garbage Collector runs
Take a look at GC running visually

‌A write simple program that creates a byte array of 100000000 elements and lets print the memory allocations.

```go
package main

import (
	"fmt"
	"runtime"
	"time"
)

func printStats(mem runtime.MemStats) {
	runtime.ReadMemStats(&mem)
	fmt.Println("mem.Alloc:", mem.Alloc)
	fmt.Println("mem.TotalAlloc:", mem.TotalAlloc)
	fmt.Println("mem.HeapAlloc:", mem.HeapAlloc)
	fmt.Println("mem.NumGC:", mem.NumGC)
	fmt.Println("-----")
}

func main() {
	var mem runtime.MemStats
	for i := 0; i < 2; i++ {
		s := make([]byte, 100000000)
		if s == nil {
			fmt.Println("Operation failed!")
		}
		printStats(mem)
	}
	time.Sleep(time.Second)
}

```
--- 

## ‌Understanding the code

There is a package in Golang standard library called runtimewhich contains alot of useful function like :

- `runtime.ReadMemStats(&mem)`
- `runtime.GC()`

Take a loot at : https://golang.org/pkg/runtime/

### func ReadMemStats
```go
func ReadMemStats(m *MemStats)
```

‌ReadMemStats populates m with memory allocator statistics.

‌The returned memory allocator statistics are up to date as of the call to ReadMemStats. This is in contrast with a heap profile, which is a snapshot as of the most recently completed garbage collection cycle.

### ‌func GC
```go
func GC()
```

‌GC runs a garbage collection and blocks the caller until the garbage collection is complete. It may also block the entire program.

---

‌Run this program using the following flag :
```
$ GODEBUG=gctrace=1 go run gc.go
```

‌Output i receive [ don’t get scared, will clean up ] :

```
// some other output //gc 3 @0.029s 5%: 0.020+2.8+0.009 ms clock, 0.081+0.12/2.7/0.19+0.036 ms cpu, 13->15->10 MB, 15 MB goal, 4 P
mem.Alloc: 100092312
mem.TotalAlloc: 100092312
mem.HeapAlloc: 100092312
mem.NumGC: 0
-----
gc 1 @0.001s 3%: 0.035+0.20+0.002 ms clock, 0.14+0.041/0.037/0.10+0.010 ms cpu, 95->95->0 MB, 96 MB goal, 4 Pmem.Alloc: 100081456mem.TotalAlloc: 100101064mem.HeapAlloc: 100081456mem.NumGC: 1
-----
gc 2 @0.021s 0%: 0.014+0.11+0.003 ms clock, 0.058+0.068/0.050/0.049+0.012 ms cpu, 95->95->0 MB, 96 MB goal, 4 P
```

‌Let look at the second run :
```
-----gc 1 @0.001s 3%: 0.035+0.20+0.002 ms clock, 0.14+0.041/0.037/0.10+0.010 ms cpu, 95->95->0 MB, 96 MB goal, 4 P
mem.Alloc: 100081456
mem.TotalAlloc: 100101064
mem.HeapAlloc: 100081456
mem.NumGC: 1
-----
gc 2 @0.021s 0%: 0.014+0.11+0.003 ms clock, 0.058+0.068/0.050/0.049+0.012 ms cpu, 95->95->0 MB, 96 MB goal, 4 P
```

‌In first line : ignoring the value from CPU profiler, take a look at `95->95>0 MB`. The first number is the heap size when the garbage collector is about to run. The second value is the heap size when the garbage collector ends its operation. The last value is the size of the live heap that is 0

It allocated `100081456` bytes of memory, more than the `100000000` becuase that is the length of slices we specified, and `100081456` bytes is the capacity of the slices or simply everytime you make a slices, some extra memory is allocated with it in case the size needs to expand in future.

Thus before running through the second iteration of the loop, it clears out the memory allocated during the first

Well thats it’s, take a look around the the runtime package or run your own programs with this compiler flag and see visually where is slows down

I hope you like this article !