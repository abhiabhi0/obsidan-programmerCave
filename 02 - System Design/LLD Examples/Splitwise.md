### **1. Key Features**

#### **Group Management**
- **Create Group**: Allow users to create groups for expense sharing.
- **Add/Remove Members**: Enable dynamic management of group participants.
- **Track Group Expenses**: Maintain a record of all expenses linked to the group.

#### **Expense Management**
- **Add Expense**: Users can add expenses specifying participants and amounts.
- **Split Expense**:
    - Equally
    - Unequally
    - By percentage
- **View Balances**: Show current balances for all users in a group.
#### **Settlement**
- **Settle Balances**: Clear debts between users by recording payments.

---

### **2. Advanced Design Patterns**

#### **Factory Pattern**
- **Purpose**: Simplifies creating different types of expenses dynamically.
- **Implementation**:
    - `ExpenseFactory` creates objects such as `EqualExpense`, `UnequalExpense`, or `PercentageExpense`.
Ref: [[08 - Creational Design Patterns - Prototype, Factory Method and Abstract Factory]]

#### **Observer Pattern**
- **Purpose**: Notify users/groups of changes, like new expenses or settlements.
- **Implementation**:
    - A central notification system subscribes users to relevant updates.

#### **Strategy Pattern**
- **Purpose**: Abstracts splitting logic for flexibility and cleaner code.
- **Implementation**:
    - Strategies include `EqualSplit`, `UnequalSplit`, or `PercentageSplit`.
Ref:  [[11 - Behavioural Design Patterns - Observer and Strategy]]
---

### **3. Class Diagram**

![[01 - Splitwise-class-diagram.png]]

---

### **4. Code Implementation**

#### **Python Code**

Here’s a Python implementation integrating both beginner-friendly and advanced design patterns.

```python
class User:
    def __init__(self, user_id, name, email):
        self.user_id = user_id
        self.name = name
        self.email = email
        self.groups = []
    
    def notify(self, message):
        print(f"Notification for {self.name}: {message}")


class Group:
    def __init__(self, group_id, group_name):
        self.group_id = group_id
        self.group_name = group_name
        self.users = []
        self.expenses = []

    def add_member(self, user):
        self.users.append(user)

    def remove_member(self, user):
        self.users.remove(user)


class Split:
    def __init__(self, user, amount):
        self.user = user
        self.amount = amount


class Expense:
    def __init__(self, expense_id, description, paid_by, amount):
        self.expense_id = expense_id
        self.description = description
        self.paid_by = paid_by
        self.amount = amount
        self.splits = []

    def calculate_splits(self):
        pass


class EqualExpense(Expense):
    def calculate_splits(self, users):
        split_amount = self.amount / len(users)
        self.splits = [Split(user, split_amount) for user in users]


class UnequalExpense(Expense):
    def calculate_splits(self, amounts):
        self.splits = [Split(user, amount) for user, amount in amounts.items()]


class PercentageExpense(Expense):
    def calculate_splits(self, percentages):
        self.splits = [
            Split(user, self.amount * percentages[user] / 100) for user in percentages
        ]


class ExpenseFactory:
    @staticmethod
    def create_expense(type, *args):
        if type == "equal":
            return EqualExpense(*args)
        elif type == "unequal":
            return UnequalExpense(*args)
        elif type == "percentage":
            return PercentageExpense(*args)
        else:
            raise ValueError("Invalid expense type")


class NotificationService:
    def __init__(self):
        self.subscribers = []

    def subscribe(self, user):
        self.subscribers.append(user)

    def unsubscribe(self, user):
        self.subscribers.remove(user)

    def notify_all(self, message):
        for user in self.subscribers:
            user.notify(message)

# Example Usage
group = Group("G1", "Trip to Paris")
user1 = User("U1", "Alice", "alice@example.com")
user2 = User("U2", "Bob", "bob@example.com")

group.add_member(user1)
group.add_member(user2)

expense = ExpenseFactory.create_expense("equal", "E1", "Lunch", user1, 100)
expense.calculate_splits(group.users)

notification_service = NotificationService()
notification_service.subscribe(user1)
notification_service.subscribe(user2)

notification_service.notify_all("New expense added!")
```

---

#### Golang Code

```go
package main

import (
	"errors"
	"fmt"
)

// User represents a user in the system
type User struct {
	UserID string
	Name   string
	Email  string
}

func (u *User) Notify(message string) {
	fmt.Printf("Notification for %s: %s\n", u.Name, message)
}

// Group represents a group of users
type Group struct {
	GroupID   string
	GroupName string
	Users     []*User
	Expenses  []Expense
}

func (g *Group) AddMember(user *User) {
	g.Users = append(g.Users, user)
}

func (g *Group) RemoveMember(user *User) {
	for i, u := range g.Users {
		if u == user {
			g.Users = append(g.Users[:i], g.Users[i+1:]...)
			break
		}
	}
}

// Split represents a share of an expense
type Split struct {
	User   *User
	Amount float64
}

// Expense is the base struct for all types of expenses
type Expense interface {
	CalculateSplits(users []*User) []Split
}

// BaseExpense contains common fields for all expenses
type BaseExpense struct {
	ExpenseID  string
	Description string
	PaidBy      *User
	Amount      float64
}

// EqualExpense splits the expense equally among users
type EqualExpense struct {
	BaseExpense
}

func (e *EqualExpense) CalculateSplits(users []*User) []Split {
	splitAmount := e.Amount / float64(len(users))
	var splits []Split
	for _, user := range users {
		splits = append(splits, Split{User: user, Amount: splitAmount})
	}
	return splits
}

// UnequalExpense splits the expense unequally
type UnequalExpense struct {
	BaseExpense
	Amounts map[*User]float64
}

func (e *UnequalExpense) CalculateSplits(users []*User) []Split {
	var splits []Split
	for user, amount := range e.Amounts {
		splits = append(splits, Split{User: user, Amount: amount})
	}
	return splits
}

// PercentageExpense splits the expense by percentages
type PercentageExpense struct {
	BaseExpense
	Percentages map[*User]float64
}

func (e *PercentageExpense) CalculateSplits(users []*User) []Split {
	var splits []Split
	for user, percentage := range e.Percentages {
		splits = append(splits, Split{User: user, Amount: e.Amount * percentage / 100})
	}
	return splits
}

// ExpenseFactory creates expenses based on type
type ExpenseFactory struct{}

func (ef *ExpenseFactory) CreateExpense(expenseType string, base BaseExpense, params interface{}) (Expense, error) {
	switch expenseType {
	case "equal":
		return &EqualExpense{BaseExpense: base}, nil
	case "unequal":
		return &UnequalExpense{BaseExpense: base, Amounts: params.(map[*User]float64)}, nil
	case "percentage":
		return &PercentageExpense{BaseExpense: base, Percentages: params.(map[*User]float64)}, nil
	default:
		return nil, errors.New("invalid expense type")
	}
}

// NotificationService handles user notifications
type NotificationService struct {
	Subscribers []*User
}

func (ns *NotificationService) Subscribe(user *User) {
	ns.Subscribers = append(ns.Subscribers, user)
}

func (ns *NotificationService) Unsubscribe(user *User) {
	for i, u := range ns.Subscribers {
		if u == user {
			ns.Subscribers = append(ns.Subscribers[:i], ns.Subscribers[i+1:]...)
			break
		}
	}
}

func (ns *NotificationService) NotifyAll(message string) {
	for _, user := range ns.Subscribers {
		user.Notify(message)
	}
}

// Main function to demonstrate usage
func main() {
	// Create users
	user1 := &User{UserID: "U1", Name: "Alice", Email: "alice@example.com"}
	user2 := &User{UserID: "U2", Name: "Bob", Email: "bob@example.com"}

	// Create a group
	group := &Group{GroupID: "G1", GroupName: "Trip to Paris"}
	group.AddMember(user1)
	group.AddMember(user2)

	// Create an expense using the factory
	factory := &ExpenseFactory{}
	base := BaseExpense{ExpenseID: "E1", Description: "Lunch", PaidBy: user1, Amount: 100}

	equalExpense, _ := factory.CreateExpense("equal", base, nil)
	splits := equalExpense.CalculateSplits(group.Users)

	// Print the splits
	fmt.Println("Splits for Equal Expense:")
	for _, split := range splits {
		fmt.Printf("User: %s, Amount: %.2f\n", split.User.Name, split.Amount)
	}

	// Notify users about the new expense
	notificationService := &NotificationService{}
	notificationService.Subscribe(user1)
	notificationService.Subscribe(user2)

	notificationService.NotifyAll("New expense added!")
}

```

