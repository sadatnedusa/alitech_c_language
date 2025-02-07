## Exercise 12456

### Write a program that reads a number that says how many integer numbers are to be stored in an array, creates an array to fit the exact size of the data and then reads in that many numbers into the array.

---

To solve **Exercise 12456**, we need to create a C program that reads an integer to determine how many numbers will be stored in an array.
Then, based on that input, we will dynamically allocate memory for the array to fit the exact number of integers. 
After that, the program will read the specified number of integers into the array.

### **Code for Exercise 12456**:

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int n;

    // Ask the user how many integers they want to store
    printf("Enter the number of integers to store: ");
    scanf("%d", &n);

    // Dynamically allocate memory for the array of 'n' integers
    int *arr = (int *)malloc(n * sizeof(int));

    // Check if the memory allocation was successful
    if (arr == NULL) {
        printf("Memory allocation failed!\n");
        return 1; // Exit the program with an error code
    }

    // Read the 'n' integers into the array
    printf("Enter %d integers: \n", n);
    for (int i = 0; i < n; i++) {
        printf("Enter integer %d: ", i + 1);
        scanf("%d", &arr[i]);
    }

    // Display the stored integers
    printf("\nYou entered the following integers:\n");
    for (int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");

    // Free the dynamically allocated memory
    free(arr);

    return 0;
}
```

### **Explanation**:

1. **Reading the Number of Integers**:
   - The program starts by asking the user how many integers they want to store. This value is stored in the variable `n`.
   
2. **Dynamically Allocating Memory**:
   - Using `malloc()`, the program dynamically allocates memory for `n` integers. This is done to create an array that exactly fits the size of the input data.
   - We check whether `malloc()` returns `NULL` to ensure that memory allocation was successful. If it fails, we print an error message and exit the program.

3. **Reading the Integers**:
   - A `for` loop is used to read `n` integers from the user, one at a time, and store them in the array `arr`.

4. **Displaying the Stored Integers**:
   - After all integers are inputted, another `for` loop is used to print out the integers that were stored in the array.

5. **Freeing the Memory**:
   - Finally, the program frees the memory allocated for the array using `free()`. This is important to avoid memory leaks.

### **Example Output**:

```
Enter the number of integers to store: 5
Enter 5 integers: 
Enter integer 1: 10
Enter integer 2: 20
Enter integer 3: 30
Enter integer 4: 40
Enter integer 5: 50

You entered the following integers:
10 20 30 40 50 
```

### **Key Points**:
- We used dynamic memory allocation (`malloc`) to ensure the array size fits the exact number of integers specified by the user.
- After reading and displaying the integers, the program frees the allocated memory to ensure no memory leaks.

This program will efficiently store and handle the specified number of integers, adjusting the array size based on user input.
