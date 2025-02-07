## Exercise 12457

### Write a program to implement the linked list as described in the notes above.

---

In **Exercise 12457**, we need to implement a linked list, which is a fundamental data structure in programming. A linked list consists of nodes, where each node contains some data and a pointer to the next node. 

Here's the program that demonstrates how to implement a singly linked list, including functions for inserting, displaying, and deleting nodes:

### **Code for Exercise 12457**: Linked List Implementation

```c
#include <stdio.h>
#include <stdlib.h>

// Define a structure for a node in the linked list
typedef struct Node {
    int data;          // Store the data
    struct Node *next; // Pointer to the next node in the list
} Node;

// Function to create a new node
Node* createNode(int data) {
    Node* newNode = (Node*)malloc(sizeof(Node)); // Allocate memory for the new node
    if (newNode == NULL) {
        printf("Memory allocation failed!\n");
        exit(1);
    }
    newNode->data = data;   // Set the node data
    newNode->next = NULL;    // Initialize next as NULL
    return newNode;
}

// Function to insert a new node at the end of the list
void insertEnd(Node** head, int data) {
    Node* newNode = createNode(data);
    if (*head == NULL) {
        *head = newNode; // If the list is empty, make the new node the head
    } else {
        Node* temp = *head;
        while (temp->next != NULL) {
            temp = temp->next; // Traverse to the last node
        }
        temp->next = newNode; // Link the last node to the new node
    }
}

// Function to display the linked list
void displayList(Node* head) {
    if (head == NULL) {
        printf("List is empty.\n");
        return;
    }
    
    Node* temp = head;
    printf("Linked List: ");
    while (temp != NULL) {
        printf("%d -> ", temp->data); // Print the data of the current node
        temp = temp->next; // Move to the next node
    }
    printf("NULL\n");
}

// Function to delete a node with a specific value
void deleteNode(Node** head, int data) {
    if (*head == NULL) {
        printf("List is empty.\n");
        return;
    }
    
    Node* temp = *head;
    Node* prev = NULL;

    // If the node to be deleted is the head node
    if (temp != NULL && temp->data == data) {
        *head = temp->next; // Change the head to the next node
        free(temp); // Free the old head
        printf("Deleted node with data: %d\n", data);
        return;
    }

    // Search for the node to be deleted
    while (temp != NULL && temp->data != data) {
        prev = temp;
        temp = temp->next;
    }

    // If the data was not found
    if (temp == NULL) {
        printf("Node with data %d not found.\n", data);
        return;
    }

    // Unlink the node from the linked list
    prev->next = temp->next;
    free(temp); // Free the memory of the deleted node
    printf("Deleted node with data: %d\n", data);
}

// Function to free all nodes in the list
void freeList(Node* head) {
    Node* temp;
    while (head != NULL) {
        temp = head;
        head = head->next;
        free(temp); // Free each node
    }
}

int main() {
    Node* head = NULL; // Initialize the head of the linked list to NULL

    // Insert some nodes into the linked list
    insertEnd(&head, 10);
    insertEnd(&head, 20);
    insertEnd(&head, 30);
    insertEnd(&head, 40);

    // Display the linked list
    displayList(head);

    // Delete a node with data 20
    deleteNode(&head, 20);

    // Display the linked list after deletion
    displayList(head);

    // Free all nodes in the linked list before exiting
    freeList(head);

    return 0;
}
```

### **Explanation**:

1. **Node Structure**: 
   - Each node in the linked list contains:
     - `data`: The value stored in the node.
     - `next`: A pointer to the next node in the list.
   
2. **`createNode` Function**: 
   - This function creates a new node, allocates memory for it, sets its data, and initializes the `next` pointer to `NULL`.

3. **`insertEnd` Function**: 
   - This function inserts a new node at the end of the linked list. It checks if the list is empty; if it is, the new node becomes the head. Otherwise, it traverses to the end of the list and appends the new node.

4. **`displayList` Function**: 
   - This function prints all the nodes in the linked list. It traverses the list from the head to the end, printing each node's data.

5. **`deleteNode` Function**: 
   - This function deletes a node with a specific data value. It checks if the node to delete is the head, or it searches for the node in the list. Once found, it removes the node and frees the allocated memory.

6. **`freeList` Function**: 
   - This function frees all the dynamically allocated memory for the nodes in the linked list.

7. **`main` Function**:
   - In the `main` function, we perform the following operations:
     - Insert values into the linked list.
     - Display the list.
     - Delete a node from the list.
     - Display the list again after deletion.
     - Free all nodes at the end of the program to prevent memory leaks.

### **Example Output**:

```
Linked List: 10 -> 20 -> 30 -> 40 -> NULL
Deleted node with data: 20
Linked List: 10 -> 30 -> 40 -> NULL
```

### **Key Points**:
- We use dynamic memory allocation (`malloc`) to create nodes for the linked list.
- The program supports insertion of nodes at the end, deletion of nodes by value, and proper memory management.
- Functions are modular and work with a pointer to the head of the list, making it easy to manipulate the linked list.

This program demonstrates the essential operations of a singly linked list, including inserting nodes, displaying the list, deleting nodes, and cleaning up memory.
