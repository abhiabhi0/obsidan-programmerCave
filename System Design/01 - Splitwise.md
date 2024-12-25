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
from abc import ABC, abstractmethod
from typing import List, Dict

# Singleton Pattern
class UserManager:
    _instance = None

    def __new__(cls, *args, **kwargs):
        if not cls._instance:
            cls._instance = super(UserManager, cls).__new__(cls, *args, **kwargs)
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

# Factory Pattern
class ExpenseFactory(ABC):
    @abstractmethod
    def create_expense(self, total_amount, participants, extra_data=None):
        pass

class EqualExpenseFactory(ExpenseFactory):
    def create_expense(self, total_amount, participants, extra_data=None):
        return EqualExpense(total_amount, participants)

class UnequalExpenseFactory(ExpenseFactory):
    def create_expense(self, total_amount, participants, extra_data=None):
        return UnequalExpense(total_amount, participants, extra_data)

class PercentageExpenseFactory(ExpenseFactory):
    def create_expense(self, total_amount, participants, extra_data=None):
        return PercentageExpense(total_amount, participants, extra_data)

# Observer Pattern
class Observer(ABC):
    @abstractmethod
    def update(self):
        pass

class ExpenseObserver(Observer):
    def update(self):
        print("Expenses updated for all users/groups.")

# Strategy Pattern
class SplitStrategy(ABC):
    @abstractmethod
    def split_expense(self, total_amount, participants, extra_data=None):
        pass

class EqualSplitStrategy(SplitStrategy):
    def split_expense(self, total_amount, participants, extra_data=None):
        split = total_amount / len(participants)
        return {user: split for user in participants}

class UnequalSplitStrategy(SplitStrategy):
    def split_expense(self, total_amount, participants, extra_data=None):
        return {user: amount for user, amount in zip(participants, extra_data)}

class PercentageSplitStrategy(SplitStrategy):
    def split_expense(self, total_amount, participants, extra_data=None):
        return {user: (percentage / 100) * total_amount for user, percentage in zip(participants, extra_data)}

# Command Pattern
class ExpenseCommand(ABC):
    @abstractmethod
    def execute(self):
        pass

class AddExpenseCommand(ExpenseCommand):
    def __init__(self, expense):
        self.expense = expense

    def execute(self):
        self.expense.calculate_shares()

# Facade Pattern
class SplitwiseFacade:
    def __init__(self):
        self.user_manager = UserManager()
        self.expenses = []
        self.observers = []

    def add_user(self, user):
        self.user_manager.add_user(user)

    def add_observer(self, observer):
        self.observers.append(observer)

    def notify_observers(self):
        for observer in self.observers:
            observer.update()

    def add_expense(self, factory, total_amount, participants, extra_data=None):
        expense = factory.create_expense(total_amount, participants, extra_data)
        command = AddExpenseCommand(expense)
        command.execute()
        self.expenses.append(expense)
        self.notify_observers()
        return expense.shares

# Expense Classes
class Expense(ABC):
    def __init__(self, total_amount, participants, strategy, extra_data=None):
        self.total_amount = total_amount
        self.participants = participants
        self.strategy = strategy
        self.extra_data = extra_data
        self.shares = {}

    @abstractmethod
    def calculate_shares(self):
        pass

class EqualExpense(Expense):
    def __init__(self, total_amount, participants):
        super().__init__(total_amount, participants, EqualSplitStrategy())

    def calculate_shares(self):
        self.shares = self.strategy.split_expense(self.total_amount, self.participants)

class UnequalExpense(Expense):
    def __init__(self, total_amount, participants, extra_data):
        super().__init__(total_amount, participants, UnequalSplitStrategy(), extra_data)

    def calculate_shares(self):
        self.shares = self.strategy.split_expense(self.total_amount, self.participants, self.extra_data)

class PercentageExpense(Expense):
    def __init__(self, total_amount, participants, extra_data):
        super().__init__(total_amount, participants, PercentageSplitStrategy(), extra_data)

    def calculate_shares(self):
        self.shares = self.strategy.split_expense(self.total_amount, self.participants, self.extra_data)

# Main Method with Examples
if __name__ == "__main__":
    # Create SplitwiseFacade and Observers
    splitwise = SplitwiseFacade()
    observer = ExpenseObserver()
    splitwise.add_observer(observer)

    # Add Users
    user1 = User("1", "Alice")
    user2 = User("2", "Bob")
    user3 = User("3", "Charlie")
    splitwise.add_user(user1)
    splitwise.add_user(user2)
    splitwise.add_user(user3)

    # Example: Equal Split
    print("Equal Split:")
    shares = splitwise.add_expense(EqualExpenseFactory(), 120.0, [user1, user2, user3])
    print(shares)

    # Example: Unequal Split
    print("\nUnequal Split:")
    shares = splitwise.add_expense(UnequalExpenseFactory(), 150.0, [user1, user2, user3], [60.0, 40.0, 50.0])
    print(shares)

    # Example: Percentage Split
    print("\nPercentage Split:")
    shares = splitwise.add_expense(PercentageExpenseFactory(), 200.0, [user1, user2, user3], [50, 30, 20])
    print(shares)

```

---

## Go Implementation

```go
package main

import (
	"fmt"
	"sync"
)

// Singleton Pattern
type UserManager struct {
	users map[string]*User
}

var instance *UserManager
var once sync.Once

func GetUserManager() *UserManager {
	once.Do(func() {
		instance = &UserManager{users: make(map[string]*User)}
	})
	return instance
}

func (um *UserManager) AddUser(user *User) {
	um.users[user.ID] = user
}

func (um *UserManager) GetUser(userID string) *User {
	return um.users[userID]
}

type User struct {
	ID   string
	Name string
}

func NewUser(id, name string) *User {
	return &User{ID: id, Name: name}
}

// Factory Pattern
type ExpenseFactory interface {
	CreateExpense(totalAmount float64, participants []*User, extraData []float64) Expense
}

type EqualExpenseFactory struct{}

func (f *EqualExpenseFactory) CreateExpense(totalAmount float64, participants []*User, extraData []float64) Expense {
	return &EqualExpense{totalAmount: totalAmount, participants: participants}
}

type UnequalExpenseFactory struct{}

func (f *UnequalExpenseFactory) CreateExpense(totalAmount float64, participants []*User, extraData []float64) Expense {
	return &UnequalExpense{totalAmount: totalAmount, participants: participants, extraData: extraData}
}

type PercentageExpenseFactory struct{}

func (f *PercentageExpenseFactory) CreateExpense(totalAmount float64, participants []*User, extraData []float64) Expense {
	return &PercentageExpense{totalAmount: totalAmount, participants: participants, extraData: extraData}
}

// Observer Pattern
type Observer interface {
	Update()
}

type ExpenseObserver struct{}

func (eo *ExpenseObserver) Update() {
	fmt.Println("Expenses updated for all users/groups.")
}

// Strategy Pattern
type SplitStrategy interface {
	SplitExpense(totalAmount float64, participants []*User, extraData []float64) map[*User]float64
}

type EqualSplitStrategy struct{}

func (s *EqualSplitStrategy) SplitExpense(totalAmount float64, participants []*User, extraData []float64) map[*User]float64 {
	shares := make(map[*User]float64)
	share := totalAmount / float64(len(participants))
	for _, user := range participants {
		shares[user] = share
	}
	return shares
}

type UnequalSplitStrategy struct{}

func (s *UnequalSplitStrategy) SplitExpense(totalAmount float64, participants []*User, extraData []float64) map[*User]float64 {
	shares := make(map[*User]float64)
	for i, user := range participants {
		shares[user] = extraData[i]
	}
	return shares
}

type PercentageSplitStrategy struct{}

func (s *PercentageSplitStrategy) SplitExpense(totalAmount float64, participants []*User, extraData []float64) map[*User]float64 {
	shares := make(map[*User]float64)
	for i, user := range participants {
		shares[user] = (extraData[i] / 100) * totalAmount
	}
	return shares
}

// Command Pattern
type ExpenseCommand interface {
	Execute()
}

type AddExpenseCommand struct {
	expense Expense
}

func (cmd *AddExpenseCommand) Execute() {
	cmd.expense.CalculateShares()
}

// Facade Pattern
type SplitwiseFacade struct {
	userManager *UserManager
	expenses    []Expense
	observers   []Observer
}

func NewSplitwiseFacade() *SplitwiseFacade {
	return &SplitwiseFacade{
		userManager: GetUserManager(),
		expenses:    []Expense{},
		observers:   []Observer{},
	}
}

func (sf *SplitwiseFacade) AddUser(user *User) {
	sf.userManager.AddUser(user)
}

func (sf *SplitwiseFacade) AddObserver(observer Observer) {
	sf.observers = append(sf.observers, observer)
}

func (sf *SplitwiseFacade) NotifyObservers() {
	for _, observer := range sf.observers {
		observer.Update()
	}
}

func (sf *SplitwiseFacade) AddExpense(factory ExpenseFactory, totalAmount float64, participants []*User, extraData []float64) map[*User]float64 {
	expense := factory.CreateExpense(totalAmount, participants, extraData)
	command := &AddExpenseCommand{expense: expense}
	command.Execute()
	sf.expenses = append(sf.expenses, expense)
	sf.NotifyObservers()
	return expense.GetShares()
}

// Expense Classes
type Expense interface {
	CalculateShares()
	GetShares() map[*User]float64
}

type BaseExpense struct {
	totalAmount  float64
	participants []*User
	shares       map[*User]float64
	strategy     SplitStrategy
	extraData    []float64
}

func (e *BaseExpense) GetShares() map[*User]float64 {
	return e.shares
}

type EqualExpense struct {
	BaseExpense
}

func (e *EqualExpense) CalculateShares() {
	e.strategy = &EqualSplitStrategy{}
	e.shares = e.strategy.SplitExpense(e.totalAmount, e.participants, nil)
}

type UnequalExpense struct {
	BaseExpense
}

func (e *UnequalExpense) CalculateShares() {
	e.strategy = &UnequalSplitStrategy{}
	e.shares = e.strategy.SplitExpense(e.totalAmount, e.participants, e.extraData)
}

type PercentageExpense struct {
	BaseExpense
}

func (e *PercentageExpense) CalculateShares() {
	e.strategy = &PercentageSplitStrategy{}
	e.shares = e.strategy.SplitExpense(e.totalAmount, e.participants, e.extraData)
}

// Main Function with Examples
func main() {
	// Create SplitwiseFacade and Observers
	splitwise := NewSplitwiseFacade()
	observer := &ExpenseObserver{}
	splitwise.AddObserver(observer)

	// Add Users
	user1 := NewUser("1", "Alice")
	user2 := NewUser("2", "Bob")
	user3 := NewUser("3", "Charlie")
	splitwise.AddUser(user1)
	splitwise.AddUser(user2)
	splitwise.AddUser(user3)

	// Example: Equal Split
	fmt.Println("Equal Split:")
	shares := splitwise.AddExpense(&EqualExpenseFactory{}, 120.0, []*User{user1, user2, user3}, nil)
	for user, share := range shares {
		fmt.Printf("%s owes %.2f\n", user.Name, share)
	}

	// Example: Unequal Split
	fmt.Println("\nUnequal Split:")
	shares = splitwise.AddExpense(&UnequalExpenseFactory{}, 150.0, []*User{user1, user2, user3}, []float64{60.0, 40.0, 50.0})
	for user, share := range shares {
		fmt.Printf("%s owes %.2f\n", user.Name, share)
	}

	// Example: Percentage Split
	fmt.Println("\nPercentage Split:")
	shares = splitwise.AddExpense(&PercentageExpenseFactory{}, 200.0, []*User{user1, user2, user3}, []float64{50, 30, 20})
	for user, share := range shares {
		fmt.Printf("%s owes %.2f\n", user.Name, share)
	}
}

```
