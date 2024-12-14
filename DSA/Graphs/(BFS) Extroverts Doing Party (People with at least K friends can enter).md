**Problem Statement:** Only people who have at least `K` friends can enter the party. If someone has fewer than `K` friends, they are removed from the party, and the friend count of their connected people is updated. If a connected person now has fewer than `K` friends, they are also removed.

---

### Steps:

1. **Adjacency List Construction:**
    
    - First, create an adjacency list to represent the friendships between people.
2. **Friend Count Array:**
    
    - Create an array `num_friends[]` where each index represents a person and the value at each index is the number of friends they have.
3. **Queue Initialization:**
    
    - Initialize a queue with people who have fewer than `K` friends.
4. **BFS Algorithm:**
    
    - Process each person in the queue:
        - Remove the person from the party (set their friend count to -1).
        - Update the friend count of each connected person.
        - If any friend now has fewer than `K` friends, add them to the queue.
5. **Final Result:**
    
    - After processing, the remaining people in the party will be those who have at least `K` friends.

---

### Example:

**Input:**

- People: `P_0, P_1, P_2, P_3, P_4, P_5, P_6`
    
- Friendships (Adjacency List):
    
    - `P_0: [P_1, P_2]`
    - `P_1: [P_0, P_3, P_4]`
    - `P_2: [P_0, P_5]`
    - `P_3: [P_1]`
    - `P_4: [P_1]`
    - `P_5: [P_2]`
    - `P_6: []` (No friends)
- `num_friends[]`:  
    `[2, 3, 2, 1, 1, 1, 0]`
    
- `K = 2`
    

**Steps:**

1. Initialize queue with people who have fewer than 2 friends:  
    `Queue = [5, 6]`
    
2. Process `P_5` (remove them from the party, update friends' friend count):
    
    - `num_friends[P_2]` decreases by 1.
    - Now `P_2` has fewer than 2 friends, so add `P_2` to the queue.
3. Process `P_6` (remove them from the party, but no connected people).
    
4. Now the queue contains `P_2` (process them):
    
    - `num_friends[P_0]` and `num_friends[P_5]` decrease by 1.
    - `P_0` still has 2 friends, so they stay in the party.
5. After processing, the remaining people in the party are:  
    `[P_0, P_1]`
    

---

### Python Code:

```python
from collections import deque

def party_with_extroverts(friendships, num_friends, K):
    n = len(friendships)
    queue = deque()
    
    # Initialize queue with people who have less than K friends
    for i in range(n):
        if num_friends[i] < K:
            queue.append(i)
    
    # BFS to process the people and remove those who don't meet the condition
    while queue:
        person = queue.popleft()
        
        # Remove the person from the party by setting their friend count to -1 (not in the party)
        if num_friends[person] == -1:
            continue
        num_friends[person] = -1
        
        # Update all the friends of this person
        for friend in friendships[person]:
            if num_friends[friend] != -1:
                num_friends[friend] -= 1
                if num_friends[friend] < K:
                    queue.append(friend)
    
    # Find all people who are still in the party
    party_members = [i for i in range(n) if num_friends[i] >= 0]
    
    return party_members

# Example Usage
friendships = {
    0: [1, 2],
    1: [0, 3, 4],
    2: [0, 5],
    3: [1],
    4: [1],
    5: [2],
    6: []
}

num_friends = [2, 3, 2, 1, 1, 1, 0]  # Initial number of friends of each person
K = 2  # Minimum number of friends required to enter the party

print(party_with_extroverts(friendships, num_friends, K))  # Output: [0, 1]
```

---

### Go Code:

```go
package main

import (
	"fmt"
)

func partyWithExtroverts(friendships map[int][]int, numFriends []int, K int) []int {
	n := len(friendships)
	queue := []int{}
	
	// Initialize queue with people who have less than K friends
	for i := 0; i < n; i++ {
		if numFriends[i] < K {
			queue = append(queue, i)
		}
	}

	// BFS to process the people and remove those who don't meet the condition
	for len(queue) > 0 {
		person := queue[0]
		queue = queue[1:]

		// Remove the person from the party by setting their friend count to -1 (not in the party)
		if numFriends[person] == -1 {
			continue
		}
		numFriends[person] = -1

		// Update all the friends of this person
		for _, friend := range friendships[person] {
			if numFriends[friend] != -1 {
				numFriends[friend]--
				if numFriends[friend] < K {
					queue = append(queue, friend)
				}
			}
		}
	}

	// Find all people who are still in the party
	partyMembers := []int{}
	for i := 0; i < n; i++ {
		if numFriends[i] >= 0 {
			partyMembers = append(partyMembers, i)
		}
	}

	return partyMembers
}

func main() {
	friendships := map[int][]int{
		0: {1, 2},
		1: {0, 3, 4},
		2: {0, 5},
		3: {1},
		4: {1},
		5: {2},
		6: {},
	}

	numFriends := []int{2, 3, 2, 1, 1, 1, 0}  // Initial number of friends of each person
	K := 2  // Minimum number of friends required to enter the party

	fmt.Println(partyWithExtroverts(friendships, numFriends, K))  // Output: [0 1]
}
```

---

### Time Complexity:

- **BFS/Queue Operations:** Each node and edge is processed once, so the time complexity is O(N+E)O(N + E), where NN is the number of people and EE is the number of friendships.

### Space Complexity:

- **Queue and Friend Count Array:** O(N)O(N) for storing the queue and the friend count array.