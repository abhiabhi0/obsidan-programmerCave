Come up with application that can perform

- Caller identification
- Call blocking
- Spam Detection
- Store users contacts
- Search for users contacts by name and number

### Use cases

[](https://github.com/gopalbala/truecaller#use-cases)

1. Users should be able to register - so they need an Account, means Account class
2. Users should be able to add contacts
3. Users should be able to import contacts
4. Users should be able to block contacts
5. Users should be able to report spam
6. Users should be able to unblock numbers
7. Users should be notified when suspected junk caller calls
8. Users should be able to identify caller when call comes
9. Users should be able to upgrade to premium plans
10. Users should be able to search for contacts by name
11. Users should be able to search for contacts by number
12. Users should be able to add business
13. Post registration and addition of contacts register with global directory.
14. Users should be able to search from global directory

### Account.java
```java
public abstract class Account {
    private String id;
    private String phoneNumber;
    private String userName;
    private String password;
    private LocalDateTime lastAccessed;
    private Tag tag;
    private Contact contact;
    private PersonalInfo personalInfo;
    private SocialInfo socialInfo;
    private Business business;
    private UserCategory userCategory;
    private Map<String, User> contacts;
    private CountingBloomFilter<String> blockedContacts;
    private Set<String> blockedSet;
    private ContactTrie contactTrie;

    public Account() {
    }

    public Account(String phoneNumber, String firstName) {
        this.phoneNumber = phoneNumber;
        this.personalInfo = new PersonalInfo(firstName);
    }

    public Account(String phoneNumber, String firstName, String lastName) {
        this(phoneNumber,firstName);
        this.personalInfo.setLastName(lastName);
    }

    public abstract void register(UserCategory userCategory, String userName, String password, String email, String phoneNumber, String countryCode, String firstName);
    public abstract void addConcat(User user) throws ContactsExceededException;
    public abstract void removeContact(String number) throws ContactDoesNotExistsException;
    public abstract void blockNumber(String number) throws BlockLimitExceededException;
    public abstract void unblockNumber(String number);
    public abstract void reportSpam(String number, String reason);
    public abstract void upgrade(UserCategory userCategory);
    public abstract boolean isBlocked(String number);
    public abstract boolean canReceive(String number);

    public abstract boolean importContacts(List<User> users);
}
```
### User.java
```java
public class User extends Account {
    public User() {
        setContactTrie(new ContactTrie());
    }
    
    public User(String phoneNumber, String firstName) {
        super(phoneNumber, firstName);
    }

    public User(String phoneNumber, String firstName, String lastName) {
        super(phoneNumber, firstName, lastName);
    }
}    
```

### PersonalInfo.java
```java
public class PersonalInfo {
    private String firstName;
    private String lastName;
    private String middleName;
    private String initials;
    private String dob;
    private Gender gender;
    private Address address;
    private String companyName;
    private String title;

    public PersonalInfo(String firstName) {
        this.setFirstName(firstName);
    }
}
```

```java
public class GlobalSpam {
    private BloomFilter<String> globalBlocked =
            BloomFilter.create(Funnels.stringFunnel(Charset.forName("UTF-8")),
                    MAX_GLOBAL_SPAM_COUNT);

    private CountingBloomFilter<String> globalSpam =
            new FilterBuilder(MAX_GLOBAL_SPAM_COUNT, .01)
                    .buildCountingBloomFilter(
                    );

    private GlobalSpam() {
    }

    public static GlobalSpam INSTANCE = new GlobalSpam();

    public void reportSpam(String spamNumber, String reportingNumber, String reason) {
        System.out.println("Send metrics here for spam Number " + spamNumber +
                        " reported " + reportingNumber + " for reason "+ reason);
                        
        if (globalSpam.getEstimatedCount(spamNumber) >= MAX_COUNT_TO_MARK_GLOBAL_BLOCKED) {
            globalBlocked.put(spamNumber);
        } else {
            globalSpam.add(spamNumber);
        }
    }

    public boolean isGlobalSpam(String number) {
        return globalSpam.contains(number);
    }

    public boolean isGloballyBlocked(String number) {
        return globalBlocked.mightContain(number);
    }
}
```
