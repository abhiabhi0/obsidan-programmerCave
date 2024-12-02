
```go
package main

import (
	"fmt"
	"sync"
)

// ItemType - The type of item in the stack.
type ItemType interface{}

// Stack - Stack of items.
type Stack struct {

	// Slice of type ItemType, it holds items in the stack.
	items []ItemType

	// rwLock for handling concurrent operations on the stack.
	rwLock sync.RWMutex
}

// New - Creates a new Stack.
func (stack *Stack) New() *Stack {

	stack.items = []ItemType{}

	return stack
}

// Push - Adds an Item to the top of the stack
func (stack *Stack) Push(t ItemType) {

	//Initialize items slice if not initialized
	if stack.items == nil {
		stack.items = []ItemType{}
	}

	// Acquire read, write lock before inserting a new item in the stack.
	stack.rwLock.Lock()

	// Performs append operation.
	stack.items = append(stack.items, t)

	// This will release read, write lock
	stack.rwLock.Unlock()
}

// Pop removes an Item from the top of the stack
func (stack *Stack) Pop() *ItemType {

	// Acquire read, write lock as items are going to modify.
	stack.rwLock.Lock()

	// Popping item from items slice.
	item := stack.items[len(stack.items)-1]

	//Adjusting the item's length accordingly
	stack.items = stack.items[0 : len(stack.items)-1]

	// Release read-write lock.
	stack.rwLock.Unlock()

	// Return last popped item
	return &item
}

// Size return size i.e. number of items present in stack.
func (stack *Stack) Size() int {
	// Acquire read lock
	stack.rwLock.RLock()

	// defer operation of unlocking.
	defer stack.rwLock.RUnlock()

	// Return length of items slice.
	return len(stack.items)
}

// All - return all items present in stack
func (stack *Stack) All() []ItemType {
	// Acquire read lock
	stack.rwLock.RLock()

	// defer operation of unlocking.
	defer stack.rwLock.RUnlock()

	// Return items slice to the caller.
	return stack.items
}

// IsEmpty - Check is stack is empty or not.
func (stack *Stack) IsEmpty() bool {
	// Acquire read lock
	stack.rwLock.RLock()
	// defer operation of unlock.
	defer stack.rwLock.RUnlock()

	return len(stack.items) == 0
}

func main() {

	stack := Stack{}
	fmt.Println(stack.All())
	stack.Push(10)
	fmt.Println(stack.All())
	stack.Push(20)
	fmt.Println(stack.All())
	stack.Push(30)
	fmt.Println(stack.All())
	stack.Push(40)
	fmt.Println(stack.All())
	stack.Push(50)
	fmt.Println(stack.All())
	stack.Push(60)
	fmt.Println(stack.All())

	stack.Pop()
	fmt.Println(stack.All())
	stack.Pop()
	fmt.Println(stack.All())
	stack.Pop()
	fmt.Println(stack.All())
	stack.Pop()
	fmt.Println(stack.All())
	stack.Pop()
	fmt.Println(stack.All())
	stack.Pop()
	fmt.Println(stack.All())

}
