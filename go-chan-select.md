> go channel selection

```go
type ReadOp struct {
	key int32
}

type WriteOp struct {
	key int32
}

func testChan() {
	reads := make(chan *ReadOp)
	writes := make(chan *WriteOp)
	go func() {
		for {
			select {
			case read := <-reads:
				fmt.Println("read, read.key => ", read.key)
			case write := <-writes:
				fmt.Println("write, write.key => ", write.key)
			}
		}
	}()
	for i := 0; i < 100; i++ {
		readOp := &ReadOp{
			key: int32(i),
		}
		reads <- readOp

		writeOp := &WriteOp{
			key: int32(i),
		}
		writes <- writeOp
	}
}
```