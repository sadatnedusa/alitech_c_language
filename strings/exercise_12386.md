## Exercise 12386: Program to Print Tomorrow's Date
### In this exercise, we will use enumerated types to represent the months and days of the week. Then, we will write a function that, given today's date, will print out tomorrow's date. We need to consider different month lengths and leap years.

#### Code for Exercise 12386:

```c
#include <stdio.h>

typedef enum { JAN = 31, FEB = 28, MAR = 31, APR = 30, MAY = 31, JUN = 30, 
               JUL = 31, AUG = 31, SEP = 30, OCT = 31, NOV = 30, DEC = 31 } Month;

typedef struct {
    int day;
    Month month;
    int year;
} Date;

// Function to check if a year is a leap year
int isLeapYear(int year) {
    if ((year % 4 == 0 && year % 100 != 0) || (year % 400 == 0)) {
        return 1;
    }
    return 0;
}

// Function to get the number of days in a month considering leap years
int getDaysInMonth(Month month, int year) {
    if (month == FEB && isLeapYear(year)) {
        return 29;
    }
    return month;
}

// Function to print tomorrow's date
void printTomorrow(Date today) {
    int daysInMonth = getDaysInMonth(today.month, today.year);

    if (today.day < daysInMonth) {
        today.day++;
    } else {
        today.day = 1;
        if (today.month == DEC) {
            today.month = JAN;
            today.year++;
        } else {
            today.month = (Month)(today.month + 1);
        }
    }

    // Print the next date
    printf("Tomorrow's date: %d", today.day);
    
    switch (today.month) {
        case JAN: printf(" January"); break;
        case FEB: printf(" February"); break;
        case MAR: printf(" March"); break;
        case APR: printf(" April"); break;
        case MAY: printf(" May"); break;
        case JUN: printf(" June"); break;
        case JUL: printf(" July"); break;
        case AUG: printf(" August"); break;
        case SEP: printf(" September"); break;
        case OCT: printf(" October"); break;
        case NOV: printf(" November"); break;
        case DEC: printf(" December"); break;
    }
    printf(" %d\n", today.year);
}

int main() {
    Date today = {31, JAN, 2025};  // Example: January 31, 2025
    printTomorrow(today);
    return 0;
}
```

### **Explanation**:
- **Enumerated Type**: `Month` is an enumerated type representing the number of days in each month.
- **Leap Year Check**: The `isLeapYear` function checks whether a given year is a leap year or not.
- **Date Calculation**: The `printTomorrow` function calculates tomorrow's date by checking if the current day is the last day of the month and adjusting accordingly.
- **Output**: The program prints tomorrow's date in the format: `Tomorrow's date: 1 February 2025`.

### **Example Output**:
```
Tomorrow's date: 1 February 2025
```

---
