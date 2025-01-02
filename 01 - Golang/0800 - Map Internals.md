### Summary:
1. **Map Header**: Metadata structure holding entry count, log(buckets), pointer to bucket array, and a random hash seed.
2. **Bucket Details**:
    - Each bucket stores up to **8 key-value pairs**.
    - Contains an array of hash codes, key-value pairs, and an overflow pointer for extra buckets.
3. **Insertion**:
    - Hash function determines the bucket.
    - If full, a new bucket is created and linked via overflow pointer.
4. **Growth**:
    - Triggered when load factor exceeds **6.5**.
    - Buckets double, old entries redistributed incrementally.
5. **Deletion**:
    - Buckets remain; map size never shrinks.

![[Pasted image 20240603131221.png]]

![[Pasted image 20240603131241.png]]

In golang maps are structured as array of buckets,

So lets see what happens when we use that famous syntax to create a map.

```
m := make(map[string]string)
```

Internally a structure called map header is created and the variable m receives a pointer to this structure, the map header contains all the meta information about the map, like:

- The number of entries that are currently in the map
- _The number of buckets in a map is always equal to power of two_ hence the log(buckets) stored to keep the value small
- Pointer to the bucket array that is stored in contiguous memory location,
- Hash seed which is random to create each map differently

Each bucket stores an array of hash codes, a list of key value pairs and an overflow pointer. The array of hash code stores hash code for each key in the bucket, this is used for faster comparison. **Each bucket can store maximum of 8 such key value pairs**. The overflow pointer can point to a new bucket if more that 8 values are received by a bucket.

### **What happens when we insert a new value in the map**

Hash function is called and a hash code is generated for the given key, based on a part of the hash code a bucket is determined to store the key-value pair. Once the bucket is selected the entry needs to be held in that bucket. The complete hash code of the incoming key is compared with all the hashes from the initial array of hash codes i.e h1, h2... if no hash code matches that means this is a new entry. Now if the bucket contains an empty slot then the new entry is stored at the end of the list of key-value pairs, else a new bucket is created and the entrance is stored in the new bucket, and the overflow pointer of the old bucket points to this new bucket.

### **What happens when the map grows**

Every time the number of elements in a bucket reaches a certain limit, i.e the load factor which is 6.5, the map will grow in size by doubling the number of buckets. This is done by creating a new bucket array consisting of twice the number of buckets than the old array, and then copying all the buckets from the old to a new array but this is done very efficiently over time and not at once. During this, the map maintains a pointer to the old bucket which is stored at the end of the new bucket array.

### **What happens when we delete an entry from the map**

Maps in golang only grow in size. Even if we delete all the entries from the map, the number of buckets will remain the same