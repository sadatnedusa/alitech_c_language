## Write a function ``replace'' which takes a pointer to a string as a parameter, which replaces all spaces in that string by minus signs, and delivers the number of spaces it replaced.

```
Thus
        char *cat = "The cat sat";
        n = replace( cat );
should set
        cat to "The-cat-sat"
and
        n to 2.
```

### C function `replace` that takes a pointer to a string, replaces all spaces with minus signs, and returns the number of spaces it replaced:

```c
#include <stdio.h>

int replace(char *str) {
    int count = 0;
    
    // Iterate through the string until we reach the null terminator
    for (int i = 0; str[i] != '\0'; i++) {
        if (str[i] == ' ') {
            str[i] = '-';  // Replace space with a minus sign
            count++;       // Increment the count of spaces replaced
        }
    }

    return count;
}

int main() {
    char cat[] = "The cat sat";
    int n = replace(cat);
    
    // Output the modified string and the number of spaces replaced
    printf("Modified string: %s\n", cat);
    printf("Number of spaces replaced: %d\n", n);
    
    return 0;
}
```

### Explanation:
- The `replace` function iterates over each character in the string.
- If the character is a space (`' '`), it replaces it with a minus sign (`'-'`) and increments the `count`.
- The function returns the total count of spaces replaced.

### Example Output:
```
Modified string: The-cat-sat
Number of spaces replaced: 2
```

This code will replace all spaces in the string with minus signs and return the number of replacements made.
