## Exercise 12458

### Write a program to sort a sequence of numbers using a binary tree (Using Pointers). A binary tree is a tree structure with only two (possible) branches from each node (Fig. 10.1). 
Each branch then represents a false or true decision. To sort numbers simply assign the left branch to take numbers less than the node number and the right branch any other number (greater than or equal to).
To obtain a sorted list simply search the tree in a depth first fashion.


![image](https://github.com/user-attachments/assets/991586fd-e0b7-4ea8-b950-163ca77c2d14)

---

In **Exercise 12458**, you are tasked with implementing a binary search tree (BST) to sort a sequence of numbers. The idea behind this is to insert numbers into the tree such that:

- For each node:
  - The left child contains numbers less than the node's value.
  - The right child contains numbers greater than or equal to the node's value.
  
Once the tree is built, a depth-first traversal (in-order traversal) will give us the numbers in sorted order.

### **Steps**:
1. **Binary Tree Node Structure**: Each node in the tree will have:
   - `data`: the number stored in the node.
   - `left`: a pointer to the left child node.
   - `right`: a pointer to the right child node.

2. **Insertion into the Tree**: 
   - If the number is less than the current node's value, it goes to the left subtree.
   - If the number is greater than or equal to the current node's value, it goes to the right subtree.

3. **In-order Traversal**: 
   - We perform a depth-first traversal to get the sorted numbers. This means we first visit the left child, then the current node, and then the right child.

### **Code for Exercise 12458**: Sorting using a Binary Search Tree (BST)

```c
#include <stdio.h>
#include <stdlib.h>

// Define a structure for the binary tree node
typedef struct Node {
    int data;          // Store the data
    struct Node *left; // Pointer to the left child
    struct Node *right; // Pointer to the right child
} Node;

// Function to create a new node
Node* createNode(int data) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    if (newNode == NULL) {
        printf("Memory allocation failed!\n");
        exit(1);
    }
    newNode->data = data;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}

// Function to insert a node into the binary search tree
Node* insert(Node* root, int data) {
    if (root == NULL) {
        return createNode(data); // If the root is NULL, create a new node
    }

    // Insert data in the appropriate place
    if (data < root->data) {
        root->left = insert(root->left, data); // Insert in the left subtree
    } else {
        root->right = insert(root->right, data); // Insert in the right subtree
    }

    return root;
}

// In-order traversal to print the sorted order
void inorderTraversal(Node* root) {
    if (root != NULL) {
        inorderTraversal(root->left);  // Visit the left subtree
        printf("%d ", root->data);     // Print the node's data
        inorderTraversal(root->right); // Visit the right subtree
    }
}

// Function to free the memory of the binary tree
void freeTree(Node* root) {
    if (root != NULL) {
        freeTree(root->left);  // Recursively free the left subtree
        freeTree(root->right); // Recursively free the right subtree
        free(root);            // Free the current node
    }
}

int main() {
    Node* root = NULL;  // Start with an empty tree
    int n, num;

    // Read how many numbers to sort
    printf("Enter the number of integers to sort: ");
    scanf("%d", &n);

    // Insert numbers into the binary search tree
    printf("Enter %d numbers:\n", n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &num);
        root = insert(root, num);  // Insert each number into the tree
    }

    // Display the sorted numbers using in-order traversal
    printf("\nSorted numbers: ");
    inorderTraversal(root);
    printf("\n");

    // Free the allocated memory for the tree
    freeTree(root);

    return 0;
}
```

### **Explanation**:

1. **Node Structure**:
   - A `Node` structure is defined, which contains:
     - `data`: The integer value stored in the node.
     - `left` and `right`: Pointers to the left and right child nodes, respectively.

2. **Create Node**:
   - The `createNode` function allocates memory for a new node, assigns the provided data, and initializes the left and right child pointers to `NULL`.

3. **Insert into the Tree**:
   - The `insert` function inserts a value into the tree. It compares the value with the current node's data:
     - If the value is less than the current node’s data, it goes into the left subtree.
     - If the value is greater than or equal to the current node’s data, it goes into the right subtree.
   - This ensures that the tree maintains the binary search property.

4. **In-order Traversal**:
   - The `inorderTraversal` function performs a depth-first traversal:
     - It recursively visits the left subtree, then processes the current node, and finally visits the right subtree.
   - This traversal prints the nodes in sorted order.

5. **Freeing the Tree**:
   - The `freeTree` function recursively frees the memory allocated for the nodes in the tree.

6. **Main Function**:
   - The main function first reads the number of integers (`n`) to be sorted and then reads `n` integers, inserting each into the binary search tree.
   - After building the tree, the program performs an in-order traversal to print the numbers in sorted order.
   - Finally, the program frees the memory allocated for the tree.

### **Example Output**:

```
Enter the number of integers to sort: 5
Enter 5 numbers:
5 3 7 1 4

Sorted numbers: 1 3 4 5 7
```

### **Key Points**:
- **Binary Search Tree**: The tree maintains the property where for each node, values in the left subtree are smaller, and values in the right subtree are greater or equal.
- **In-order Traversal**: This traversal gives the numbers in sorted order because it processes the left subtree, the current node, and then the right subtree.
- **Dynamic Memory Allocation**: The program uses `malloc` to create new nodes and `free` to deallocate memory when the tree is no longer needed.

This implementation demonstrates how to use a binary search tree to sort numbers efficiently.
