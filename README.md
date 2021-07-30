# Squeue - Slice-based queue

## Installation

You can install this package in your project by running:

```bash
go get github.com/jwhiteside11/squeue
```

and then use it in your project like so:

```go
import "github.com/jwhiteside11/squeue"

func someFunctionName() {
	queue := squeue.New()

	queue.Enqueue("Hello")
	queue.Enqueue(2)
	queue.Enqueue("The")
	queue.Enqueue("World")

	fmt.Println(queue.Peek())
	
	for _, v := range queue.Each() {
		fmt.Println(v)
	}

	for !queue.Empty() {
		queue.Dequeue()
	}
	_, err := queue.Dequeue()
	if err != nil {
		fmt.Println(err)
	}
}
```

## Available methods

- **New() Squeue {}** - Create a queue
- **Enqueue(elem interface{})** - Add element to back of queue
- **Peek () (interface{}, error)** - Retrieve, but do not remove, element from head of queue
- **Dequeue() (interface{}, error)** - Remove element from head of queue
- **Size() int** - Get size of queue
- **Empty() bool** - Returns true if queue is empty
- **Each() []interface{}** - Returns a new slice pointing containing only the underlying slice values; convinience method
- **String() string** - String representation of underlying slice values

## Performance

The slice-based queue implementation is more performant than a linked list queue, in both time and memory. The total allocation requirement will be similar between the two, but the squeue is generally over twice as fast.

This is because the squeue amortizes the time cost of allocation by growing and shrinking the underlying slice as needed. This behavior allows for quicker writes to the data structure, because we don't need to allocate anything in order to add an element. The linked list queue must spend time allocating the element each time one is added.

The memory concerns present in a slice-based queue implementation have been addressed in this module. Unused values are discarded, and the underlying slice reallocates periodically as the queue gets used. The former saves on total allocation, the latter tells the GC that we are no longer using the slice's underlying array. This way, the queue will not leak memory over time.

## Contributions

I'm open to any and all contributions. Make a PR.