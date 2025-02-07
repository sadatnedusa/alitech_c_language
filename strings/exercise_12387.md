### **Exercise 12387: Simple Database Program**

In this exercise, we need to create a simple database program that stores a person's details such as age, date of birth, address, etc. We'll define a `Person` structure to store the details and provide functions to input and display these details.

#### Code for Exercise 12387:

```c
#include <stdio.h>
#include <string.h>

#define MAX_NAME_LENGTH 100
#define MAX_ADDRESS_LENGTH 200

typedef struct {
    char name[MAX_NAME_LENGTH];
    int age;
    char dob[MAX_NAME_LENGTH]; // Date of Birth in "dd-mm-yyyy" format
    char address[MAX_ADDRESS_LENGTH];
} Person;

// Function to input person's details
void inputPersonDetails(Person *person) {
    printf("Enter name: ");
    fgets(person->name, MAX_NAME_LENGTH, stdin);
    person->name[strcspn(person->name, "\n")] = '\0';  // Remove trailing newline

    printf("Enter age: ");
    scanf("%d", &person->age);
    getchar();  // Consume the leftover newline character

    printf("Enter date of birth (dd-mm-yyyy): ");
    fgets(person->dob, MAX_NAME_LENGTH, stdin);
    person->dob[strcspn(person->dob, "\n")] = '\0';  // Remove trailing newline

    printf("Enter address: ");
    fgets(person->address, MAX_ADDRESS_LENGTH, stdin);
    person->address[strcspn(person->address, "\n")] = '\0';  // Remove trailing newline
}

// Function to display person's details
void displayPersonDetails(Person person) {
    printf("\n--- Person Details ---\n");
    printf("Name: %s\n", person.name);
    printf("Age: %d\n", person.age);
    printf("Date of Birth: %s\n", person.dob);
    printf("Address: %s\n", person.address);
}

int main() {
    Person person;

    inputPersonDetails(&person);
    displayPersonDetails(person);

    return 0;
}
```

### **Explanation**:
- **Person Structure**: The `Person` structure holds information about a person's name, age, date of birth, and address.
- **Input Function**: `inputPersonDetails` prompts the user to enter a name, age, date of birth, and address, and stores these values in the structure.
- **Display Function**: `displayPersonDetails` prints the entered details.
- **Main Function**: In the `main` function, the program collects and displays a person's details.

### **Example Output**:

```
Enter name: John Doe
Enter age: 30
Enter date of birth (dd-mm-yyyy): 15-06-1994
Enter address: 123 Main Street

--- Person Details ---
Name: John Doe
Age: 30
Date of Birth: 15-06-1994
Address: 123 Main Street
```

---

### Key Points:
- **Exercise 12386** uses enumerated types to handle months and days, calculates tomorrow's date, and considers leap years.
- **Exercise 12387** creates a simple database using structures to store a person's details and functions to handle input and output.

These programs provide a good foundation for managing dates and creating simple databases in C. Let me know if you need further explanations or improvements!
