- The problem is similar to [“Segregate 0s and 1s in an array”](https://www.geeksforgeeks.org/segregate-0s-and-1s-in-an-array-by-traversing-array-once/).
- __The problem was posed with three colors, here `0′, `1′ and `2′. The array is divided into four sections:__ 
    - arr[1] to arr[low – 1]
    - arr[low] to arr[mid – 1]
    - arr[mid] to arr[high – 1]
    - arr[high] to arr[n]
- If the ith element is 0 then swap the element to the low range.
- Similarly, if the element is 1 then keep it as it is.
- If the element is 2 then swap it with an element in high range.

****arr[] = {0, 1, 2, 0, 1, 2}****

****lo**** = 0, ****mid**** = 0, ****hi**** = 5

![](https://media.geeksforgeeks.org/wp-content/uploads/20220804130128/0th.png)

****Step – 1:**** arr[mid] == 0

- swap(arr[lo], arr[mid])
- lo = lo + 1 = 1
- mid = mid + 1 = 1
- arr[] = {0, 1, 2, 0, 1, 2}

![](https://media.geeksforgeeks.org/wp-content/uploads/20220804130128/1st.png)

****Step – 2:**** arr[mid] == 1

- mid = mid + 1 = 2
- arr[] = {0, 1, 2, 0, 1, 2}

![](https://media.geeksforgeeks.org/wp-content/uploads/20220804131120/2nd.png)

****Step – 3:**** arr[mid] == 2

- swap(arr[mid], arr[hi])
- hi = hi – 1 = 4
- arr[] = {0, 1, 2, 0, 1, 2}

![](https://media.geeksforgeeks.org/wp-content/uploads/20220804131121/3rd.png)

****Step – 4:**** arr[mid] == 2

- swap(arr[mid], arr[hi])
- hi = hi – 1 = 3
- arr[] = {0, 1, 1, 0, 2, 2}

![](https://media.geeksforgeeks.org/wp-content/uploads/20220804131121/4th.png)

****Step – 5:**** arr[mid] == 1

- mid = mid + 1 = 3
- arr[] = {0, 1, 1, 0, 2, 2}

![](https://media.geeksforgeeks.org/wp-content/uploads/20220804131121/5th.png)

****Step – 6:**** arr[mid] == 0

- swap(arr[lo], arr[mid])
- lo = lo + 1 = 2
- mid = mid + 1 = 4
- arr[] = {0, 0, 1, 1, 2, 2}

![](https://media.geeksforgeeks.org/wp-content/uploads/20220804131121/6th.png)

Hence, ****arr[] = {0, 0, 1, 1, 2, 2}****

Follow the steps below to solve the given problem:

- Keep three indices low = 1, mid = 1, and high = N and there are four ranges, 1 to low (the range containing 0), low to mid (the range containing 1), mid to high (the range containing unknown elements) and high to N (the range containing 2).
- Traverse the array from start to end and mid is less than high. (Loop counter is i)
- If the element is 0 then swap the element with the element at index low and update low = low + 1 and mid = mid + 1
- If the element is 1 then update mid = mid + 1
- If the element is 2 then swap the element with the element at index high and update high = high – 1 and update i = i – 1. As the swapped element is not processed
- Print the array.

```cpp
// Function to sort the input array,
// the array is assumed
// to have values in {0, 1, 2}
void sort012(int a[], int arr_size)
{
	int lo = 0;
	int hi = arr_size - 1;
	int mid = 0;

	// Iterate till all the elements
	// are sorted
	while (mid <= hi) {
		switch (a[mid]) {

		// If the element is 0
		case 0:
			swap(a[lo++], a[mid++]);
			break;

		// If the element is 1 .
		case 1:
			mid++;
			break;

		// If the element is 2
		case 2:
			swap(a[mid], a[hi--]);
			break;
		}
	}
}
