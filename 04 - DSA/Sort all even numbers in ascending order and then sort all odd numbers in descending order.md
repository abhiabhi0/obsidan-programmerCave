### Method 1 (Using Partition)  

```cpp
// To do two way sort. First sort even numbers in 
// ascending order, then odd numbers in descending 
// order. 
void twoWaySort(int arr[], int n) 
{ 
	
	// Current indexes from left and right 
	int l = 0, r = n - 1; 

	// Count of odd numbers 
	int k = 0; 

	while (l < r) 
	{ 
	
		// Find first even number 
		// from left side. 
		while (arr[l] % 2 != 0) 
		{ 
			l++; 
			k++; 
		} 

		// Find first odd number 
		// from right side. 
		while (arr[r] % 2 == 0 && l < r) 
			r--; 

		// Swap even number present on left and odd 
		// number right. 
		if (l < r) 
			swap(arr[l], arr[r]); 
	} 

	// Sort odd number in descending order 
	sort(arr, arr + k, greater<int>()); 

	// Sort even number in ascending order 
	sort(arr + k, arr + n); 
} 
```
1. [Partition the input array](https://www.geeksforgeeks.org/hoares-vs-lomuto-partition-scheme-quicksort/) such that all odd elements are moved to the left and all even elements on right. This step takes O(n).
2. Once the array is partitioned, sort left and right parts individually. This step takes O(n Log n).

Follow the below steps to implement the above idea:

- Initialize two variables ****l**** and ****r**** to ****0**** and ****n-1**** respectively.
- Initialize a variable ****k**** to ****0**** which will count the number of odd elements in the array.
- Run a loop while ****l < r****:  
                  a. Find the first even number from the left side of the array by checking if ****arr[l]**** is even, if not increment ****l**** and ****k**** by 1.  
                  b. Find the first odd number from the right side of the array by checking if ****arr[r]**** is odd, if not decrement ****r**** by 1.  
                  c. Swap the even number at index ****l**** with the odd number at index ****r****.
- Sort the first ****k**** elements of the array in descending order using the in-built sort function with greater****<int>()**** as the comparator.
- Sort the remaining ****n-k**** elements of the array in ascending order using the in-built sort function.
- The sorted array is now ready to be printed.

### Method 2 (Using negative multiplication) :  

1. Make all odd numbers negative.
2. Sort all numbers.
3. Revert the changes made in step 1 to get original elements back.