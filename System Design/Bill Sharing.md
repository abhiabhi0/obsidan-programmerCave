### User.java
```java
public class User {
    private String name;
    private String userId;
    private String emailId;
    private String phoneNumber;

    public User(@NonNull String emailId, String name, String phoneNumber) {
        userId = UUID.randomUUID().toString();
        this.emailId = emailId;
        this.name = name;
        this.phoneNumber = phoneNumber;
    }
}
```

### Expense.java
```java
public class Expense {
    private String id;
    private String userId;
    private String title;
    private String description;
    private LocalDateTime expenseDate;
    private ExpenseStatus expenseStatus;
    private double expenseAmount;
    private ExpenseGroup expenseGroup;
}
```

### ExpenseGroup.java
```java
public class ExpenseGroup {
    private Set<User> groupMembers = new HashSet<>();
    public ExpenseGroup() {
        this.expenseGroupId = UUID.randomUUID().toString();
        this.groupMembers = new HashSet<>();
        this.userContributions = new HashMap<>();
    }
    private String expenseGroupId;
    @Setter
    private Map<String, UserShare> userContributions;
    public void ExpenseGroup() {
    }
}
```

### UserShare.java
```java
public class UserShare {
    public UserShare(String userId, double share) {
        this.userId = userId;
        this.share = share;
        contributions = new ArrayList<>();
    }

    private String userId;
    private double share;
    List<Contribution> contributions;
}
```

### ExpenseStatus.java
```java
public enum ExpenseStatus {
    CREATED,
    PENDING,
    SETTLED
}
```
