## Key Requirements

### Functional Requirements:

1. **User Management**
    - Add user
    - Remove user
    - Fetch user balance
    
1. **Expense Management**
    - Add expense (Equally, Unequally, Percentage-based)
    - Split expenses between users
    - Fetch group balances
    
1. **Group Management**
    - Create group
    - Add/Remove members to/from group
    - Track group expenses
    
1. **Settlement**
    - Simplify and minimize transactions for debt settlement.

### Non-Functional Requirements:

- Scalability
- Concurrency handling
- Data consistency
- Fault tolerance
- High availability

---

## System Design Overview

### Design Patterns Used:

1. **Singleton Pattern** - To manage a single instance of UserManager or ExpenseManager.
2. **Factory Pattern** - To create different types of expense splits (Equal, Unequal, Percentage).
3. **Strategy Pattern** - To implement splitting strategies for different expense types.
4. **Observer Pattern** - To notify users/groups about expense updates.
5. **Command Pattern** - To encapsulate actions for adding expenses and settlements.
6. **Facade Pattern** - To provide a simple interface for managing complex operations.

---

## Class Diagram

![[Splitwise-design-diagram.png]]

---

## Key Components

### 1. **UserManager** (Singleton)

- Manages users and their balances.
- Ensures a single instance is used for user operations.

### 2. **ExpenseFactory** (Factory Pattern)

- Creates different expense objects based on the splitting strategy (Equal, Unequal, Percentage).

### 3. **SplitStrategy** (Strategy Pattern)

- Defines the logic for splitting expenses.
    - **EqualSplitStrategy**: Divides the amount equally among users.
    - **UnequalSplitStrategy**: Divides the amount based on specified shares.
    - **PercentageSplitStrategy**: Divides the amount based on specified percentages.

### 4. **Observer Pattern**

- Notifies all relevant users or groups about expense updates.

---

## Algorithms

### 1. **Balancing Algorithm**

Used to minimize the number of transactions required for settling debts.

1. Represent users and their balances.
2. Maintain separate lists for positive and negative balances.
3. Use a greedy algorithm to match positive and negative balances until all debts are settled.

### 2. **Transaction Simplification**

1. Represent balances as a graph where nodes are users and edges are debts.
2. Apply the min-cut algorithm to reduce the number of edges (transactions).

---

## Python Implementation

```python
class User:
    def __init__(self, user_id, name):
        self.id = user_id
        self.name = name
        self.balances = {}

    def add_balance(self, user_id, amount):
        if user_id in self.balances:
            self.balances[user_id] += amount
        else:
            self.balances[user_id] = amount

    def get_balance(self):
        return self.balances

class Expense:
    def __init__(self, amount, paid_by, splits, strategy):
        self.amount = amount
        self.paid_by = paid_by
        self.splits = splits
        self.strategy = strategy

    def calculate_splits(self):
        return self.strategy.calculate_splits(self.amount, self.splits)

class EqualSplitStrategy:
    def calculate_splits(self, amount, users):
        split_amount = amount / len(users)
        return {user.id: split_amount for user in users}

# Add other strategy classes similarly

# Example Usage
users = [User("1", "Alice"), User("2", "Bob")]
strategy = EqualSplitStrategy()
expense = Expense(100, users[0], users, strategy)
print(expense.calculate_splits())
```

---

## Golang Implementation

```go
package main

import (
	"fmt"
)

type User struct {
	ID       string
	Name     string
	Balances map[string]float64
}

func (u *User) AddBalance(userID string, amount float64) {
	if _, exists := u.Balances[userID]; exists {
		u.Balances[userID] += amount
	} else {
		u.Balances[userID] = amount
	}
}

func (u *User) GetBalance() map[string]float64 {
	return u.Balances
}

type SplitStrategy interface {
	CalculateSplits(amount float64, users []*User) map[string]float64
}

type EqualSplitStrategy struct {}

func (e *EqualSplitStrategy) CalculateSplits(amount float64, users []*User) map[string]float64 {
	splits := make(map[string]float64)
	splitAmount := amount / float64(len(users))
	for _, user := range users {
		splits[user.ID] = splitAmount
	}
	return splits
}

func main() {
	users := []*User{
		{ID: "1", Name: "Alice", Balances: make(map[string]float64)},
		{ID: "2", Name: "Bob", Balances: make(map[string]float64)},
	}
	strategy := &EqualSplitStrategy{}
	splits := strategy.CalculateSplits(100, users)
	fmt.Println(splits)
}
```

---

## Summary

This design includes robust patterns (Singleton, Factory, Strategy, etc.) to ensure scalability, extensibility, and maintainability of an expense-sharing application like Splitwise. Both Python and Golang implementations cover the foundational structure for managing users, expenses, and splits efficiently.