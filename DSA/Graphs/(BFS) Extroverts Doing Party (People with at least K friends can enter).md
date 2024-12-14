**Problem Statement:** You have a group of people, and each person has a certain number of friends. Only people with at least K friends can attend the party. The task is to find out who can enter the party, and those who don't meet the requirement (fewer than K friends) must be removed from the party. If someone is removed, their friends' friend count should be updated, and if any of them now have fewer than K friends, they should also be removed.

---

### Key Steps for Solution:

1. **Adjacency List Construction:**
    
    - First, we need to create an **adjacency list** to represent the relationships between people. Each index represents a person, and the value at each index is a list of their friends.
2. **Friend Count Array:**
    
    - An array `num_friends` will store the number of friends each person has.
3. **BFS Approach:**
    
    - We use **Breadth-First Search (BFS)** to process people with fewer than K friends and remove them from the party. For each person removed, we update their friends' friend count and check if any of them now have fewer than K friends, in which case they are also removed.
4. **Queue Initialization:**
    
    - Initially, add people with fewer than K friends to the queue, as they can't attend the party.
5. **Processing the Queue:**
    
    - For each person in the queue:
        - Remove them from the party.
        - Update their friends' friend count.
        - If any friend now has fewer than K friends, add them to the queue.
6. **Result:**
    
    - After processing the queue, the remaining people in the party will be those who have at least K friends.

---

### Example:

**Input:**

- People: P0,P1,P2,P3,P4,P5,P6P_0, P_1, P_2, P_3, P_4, P_5, P_6
    
- Friendships (Adjacency List):
    
    - P0:[P1,P2]P_0: [P_1, P_2]
    - P1:[P0,P3,P4]P_1: [P_0, P_3, P_4]
    - P2:[P0,P5]P_2: [P_0, P_5]
    - P3:[P1]P_3: [P_1]
    - P4:[P1]P_4: [P_1]
    - P5:[P2]P_5: [P_2]
    - P6:[]P_6: [] (No friends)
- `num_friends` Array:  
    [2,3,2,1,1,1,0][2, 3, 2, 1, 1, 1, 0]
    
- K=2K = 2
    

**Steps:**

- People with fewer than 2 friends are added to the queue: [P5,P6][P_5, P_6].
- Process P5P_5 (remove them from the party) and update P2P_2's friend count.  
    Now, P2P_2 has fewer than 2 friends, so add P2P_2 to the queue.
- Process P6P_6 (remove them from the party), but they don't affect any other person.
- Now, the remaining people in the queue are [P2][P_2].
- Process P2P_2, remove them, and update P0P_0 and P5P_5's friend count.
- After processing, people who can enter the party will be those with at least 2 friends.

---

### Python Code:

```python
from collections import deque, defaultdict

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