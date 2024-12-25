## Features

### 1. User Management
- **Create User**
- **Add/Remove User**

### 2. Group Management
- **Create Group**
- **Add/Remove Members**
- **Track Group Expenses**

### 3. Expense Management
- **Add Expense**
- **Split Expense (Equally, Unequally, by Percentage)**
- **View Balances**

### 4. Settlement
- **Settle Balances**

---

## Design Patterns Involved

### 1. Singleton Pattern
Used for a centralized **UserManager** to manage user-related operations.

### 2. Factory Pattern
Used to create different types of expenses (e.g., `EqualExpense`, `UnequalExpense`, `PercentageExpense`).

### 3. Observer Pattern
Used to notify users or groups about changes in expenses or settlements.

### 4. Strategy Pattern
Used for splitting expenses based on the desired approach (e.g., equally, unequally, or by percentage).

### 5. Command Pattern
Encapsulates expense operations like adding or modifying expenses.

### 6. Facade Pattern
Provides a simplified interface for managing users, expenses, and settlements.

---

## High-Level Architecture Diagram (HLD)

![[01 - Splitwise-hld.png]]
---

## Sequence Diagram

![[01 - Splitwise-sequence-diagram.png]]
---

## Python Implementation

```python
class UserManager:
    _instance = None

    def __new__(cls, *args, **kwargs):
        if not cls._instance:
            cls._instance = super(UserManager, cls).__new__(cls)
            cls._instance.users = {}
        return cls._instance

    def add_user(self, user):
        self.users[user.user_id] = user

    def get_user(self, user_id):
        return self.users.get(user_id)

class User:
    def __init__(self, user_id, name):
        self.user_id = user_id
        self.name = name

class Expense:
    def __init__(self, total_amount, participants, split_strategy):
        self.total_amount = total_amount
        self.participants = participants
        self.split_strategy = split_strategy
        self.shares = {}

    def calculate_shares(self):
        self.shares = self.split_strategy.split(self.total_amount, self.participants)

class SplitStrategy:
    def split(self, total_amount, participants):
        raise NotImplementedError

class EqualSplit(SplitStrategy):
    def split(self, total_amount, participants):
        share = total_amount / len(participants)
        return {participant: share for participant in participants}

class ExpenseFactory:
    @staticmethod
    def create_expense(expense_type, total_amount, participants):
        if expense_type == "equal":
            return Expense(total_amount, participants, EqualSplit())
        # Additional expense types can be added here.

class Observer:
    def update(self):
        pass

class ExpenseObserver(Observer):
    def update(self):
        print("Balances updated!")

class SplitwiseFacade:
    def __init__(self):
        self.user_manager = UserManager()
        self.expenses = []
        self.observers = []

    def add_observer(self, observer):
        self.observers.append(observer)

    def notify_observers(self):
        for observer in self.observers:
            observer.update()

    def add_user(self, user):
        self.user_manager.add_user(user)

    def add_expense(self, expense_type, total_amount, participants):
        expense = ExpenseFactory.create_expense(expense_type, total_amount, participants)
        expense.calculate_shares()
        self.expenses.append(expense)
        self.notify_observers()
```

---

## Go Implementation

```go
package main

import (
	"fmt"
)

type UserManager struct {
	users map[string]*User
}

var instance *UserManager

func GetUserManager() *UserManager {
	if instance == nil {
		instance = &UserManager{users: make(map[string]*User)}
	}
	return instance
}

type User struct {
	UserID string
	Name   string
}

type SplitStrategy interface {
	Split(totalAmount float64, participants []*User) map[*User]float64
}

type EqualSplit struct{}

func (es *EqualSplit) Split(totalAmount float64, participants []*User) map[*User]float64 {
	shares := make(map[*User]float64)
	share := totalAmount / float64(len(participants))
	for _, participant := range participants {
		shares[participant] = share
	}
	return shares
}

type Expense struct {
	TotalAmount  float64
	Participants []*User
	Shares       map[*User]float64
	Strategy     SplitStrategy
}

func (e *Expense) CalculateShares() {
	e.Shares = e.Strategy.Split(e.TotalAmount, e.Participants)
}

func main() {
	manager := GetUserManager()
	user1 := &User{UserID: "1", Name: "Alice"}
	user2 := &User{UserID: "2", Name: "Bob"}
	manager.users[user1.UserID] = user1
	manager.users[user2.UserID] = user2

	expense := &Expense{
		TotalAmount:  100.0,
		Participants: []*User{user1, user2},
		Strategy:     &EqualSplit{},
	}
	expense.CalculateShares()
	fmt.Println(expense.Shares)
}
```
