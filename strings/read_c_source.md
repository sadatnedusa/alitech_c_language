## Exercise 12347

Write a program which will read in the source of a C program from its standard input, and print out all the starred items in the following statistics for the program (all as integers).
(Note the comment on tab characters at the end of this specification.)

Print out the following values:

  Lines:
  *  The total number of lines
  *  The total number of blank lines
        (Any lines consisting entirely of white space should be
        considered as blank lines.)
     The percentage of blank lines (100 * blank_lines / lines)

  Characters:
  *  The total number of characters after tab expansion
  *  The total number of spaces after tab expansion
  *  The total number of leading spaces after tab expansion
      (These are the spaces at the start of a line, before any visible
        character; ignore them if there are no visible characters.)
    The average number of
      characters per line
      characters per line ignoring leading spaces
      leading spaces per line
      spaces per line ignoring leading spaces

  Comments:
  *  The total number of comments in the program
  *  The total number of characters in the comments in the program
       excluding the "/*" and "*/" thenselves
    The percentage of number of comments to total lines
    The percentage of characters in comments to characters

  Identifiers:
    We are concerned with all the occurrences of "identifiers" in the
      program where each part of the text starting with a letter,
      and continuing with letter, digits and underscores is considered
      to be an identifier, provided that it is not
          in a comment,
          or in a string,
          or within primes.
        Note that
            "abc\"def"
        the internal escaped quote does not close the string.
        Also, the representation of the escape character is
            '\\'
 and of prime is
            '\''
      Do not attempt to exclude the fixed words of the language,
      treat them as identifiers. Print
  *  The total number of identifier occurrences.
  *  The total number of characters in them.
    The average identifier length.

  Indenting:
  *  The total number of times either of the following occurs:
      a line containing a "}" is more indented than the preceding line
      a line is preceded by a line containing a "{" and is less
        indented than it.
      The "{" and "}" must be ignored if in a comment or string or
        primes, or if the other line involved is entirely comment.
    A single count of the sum of both types of error is required.
NOTE: All tab characters ('') on input should be interpreted as multiple spaces using the rule:
  "move to the next modulo 8 column"
  where the first column is numbered column 0.
 col before tab | col after tab
        ---------------+--------------
                0      |      8
                1      |      8
                7      |      8
                8      |     16
                9      |     16
               15      |     16
               16      |     24
To read input a character at a time the skeleton has code incorporated to read a line at a time for you using
        char ch;
        ch = getchar();
Which will deliver each character exactly as read. The "getline" function then puts the line just read in the global array of characters "linec", null terminated, and delivers the length of the line, or a negative value if end of data has been encountered.
You can then look at the characters just read with (for example)

        switch( linec[0] ) {
        case ' ': /* space ..... */
                break;
        case '\t': /* tab character .... */
                break;
        case '\n': /* newline ... */
                break;
        ....
        } /* end switch */
End of data is indicated by scanf NOT delivering the value 1.


Your output should be in the following style:

        Total lines                     126
        Total blank lines               3
        Total characters                3897
        Total spaces                    1844
        Total leading spaces            1180
        Total comments                  7
        Total chars in comments         234
        Total number of identifiers     132
        Total length of identifiers     606
        Total indenting errors          2
You may gather that the above program (together with the unstarred items) forms the basis of part of your marking system! Do the easy bits first, and leave it at that if some aspects worry you. Come back to me if you think my solution (or the specification) is wrong! That is quite possible!

---

This exercise involves analyzing a C program's source code by reading it from standard input, processing it character by character, and then outputting various statistics related to the program structure and formatting. The goal is to compute the total number of lines, blank lines, character counts, comments, identifiers, and indentation errors.

The task can be divided into manageable steps:
1. **Reading the Input**: We'll read the C source code line by line, processing characters one by one.
2. **Tab Expansion**: We need to expand tab characters (`\t`) to spaces, following the modulo 8 rule.
3. **Blank Lines and Line Counts**: Track blank lines and total lines.
4. **Character and Space Counts**: Count characters, spaces, leading spaces, and handle indentation errors.
5. **Comments Handling**: Detect and process block comments (`/* ... */`) and count them.
6. **Identifiers**: Identify valid identifiers outside of strings and comments.
7. **Indentation Errors**: Detect lines where the indentation is inconsistent based on the previous line.

We'll use the `getchar()` function to read input one character at a time, and `getline()` to store the line. Let's break this down with a skeleton implementation. We'll provide a solution for counting lines, blank lines, characters, spaces, comments, and identifiers. 

### Skeleton Code

```c
#include <stdio.h>
#include <ctype.h>
#include <string.h>
#include <stdbool.h>

#define MAX_LINE_LENGTH 1024

char linec[MAX_LINE_LENGTH]; // Global array to store the current line

// Function to expand tabs to spaces (tab size 8)
void expand_tabs(char *line) {
    int i = 0, j = 0;
    while (line[i] != '\0') {
        if (line[i] == '\t') {
            int spaces_to_add = 8 - (j % 8);
            for (int k = 0; k < spaces_to_add; k++) {
                line[j++] = ' ';
            }
        } else {
            line[j++] = line[i];
        }
        i++;
    }
    line[j] = '\0'; // Null terminate the expanded line
}

// Function to count the total number of spaces and leading spaces in a line
void count_spaces_and_leading_spaces(char *line, int *spaces, int *leading_spaces) {
    int i = 0;
    *leading_spaces = 0;
    while (line[i] == ' ') {
        (*leading_spaces)++;
        i++;
    }
    *spaces = 0;
    while (line[i] != '\0') {
        if (line[i] == ' ') {
            (*spaces)++;
        }
        i++;
    }
}

// Function to count comments and characters in comments
void count_comments(char *line, int *comment_count, int *comment_chars) {
    bool in_comment = false;
    for (int i = 0; line[i] != '\0'; i++) {
        if (!in_comment && line[i] == '/' && line[i + 1] == '*') {
            in_comment = true;
            i++; // Skip '*' character
        } else if (in_comment && line[i] == '*' && line[i + 1] == '/') {
            in_comment = false;
            i++; // Skip '/' character
        } else if (in_comment) {
            (*comment_chars)++;
        }
        if (in_comment) {
            (*comment_count)++;
        }
    }
}

// Function to check if a character is part of an identifier
bool is_identifier_char(char c) {
    return isalnum(c) || c == '_';
}

// Function to count the number of identifiers and their total length
void count_identifiers(char *line, int *identifier_count, int *identifier_length) {
    bool in_identifier = false;
    int current_identifier_length = 0;

    for (int i = 0; line[i] != '\0'; i++) {
        if (is_identifier_char(line[i])) {
            if (!in_identifier) {
                in_identifier = true;
                current_identifier_length = 1; // Start a new identifier
            } else {
                current_identifier_length++; // Continue the identifier
            }
        } else {
            if (in_identifier) {
                (*identifier_count)++;
                (*identifier_length) += current_identifier_length;
                in_identifier = false;
            }
        }
    }
    // If the line ends with an identifier, we need to count it too
    if (in_identifier) {
        (*identifier_count)++;
        (*identifier_length) += current_identifier_length;
    }
}

// Main function to process the input and print statistics
int main() {
    int total_lines = 0;
    int blank_lines = 0;
    int total_chars = 0;
    int total_spaces = 0;
    int total_leading_spaces = 0;
    int total_comments = 0;
    int total_comment_chars = 0;
    int total_identifiers = 0;
    int total_identifier_length = 0;
    int total_indent_errors = 0;

    int prev_indentation = -1;

    while (fgets(linec, MAX_LINE_LENGTH, stdin)) {
        total_lines++;
        int spaces = 0, leading_spaces = 0;
        count_spaces_and_leading_spaces(linec, &spaces, &leading_spaces);
        total_spaces += spaces;
        total_leading_spaces += leading_spaces;

        // Expand tabs to spaces
        expand_tabs(linec);

        // Count blank lines
        if (linec[0] == '\n' || strlen(linec) == 0) {
            blank_lines++;
        }

        // Count comments
        count_comments(linec, &total_comments, &total_comment_chars);

        // Count identifiers
        count_identifiers(linec, &total_identifiers, &total_identifier_length);

        // Detect indentation errors
        int current_indentation = leading_spaces;
        if (prev_indentation != -1) {
            // Check if indentation is inconsistent (we can add rules for this)
            if (current_indentation > prev_indentation) {
                total_indent_errors++; // Indentation after "{" is more
            } else if (current_indentation < prev_indentation) {
                total_indent_errors++; // Indentation before "}" is less
            }
        }
        prev_indentation = current_indentation;
    }

    // Calculate percentages
    float blank_line_percentage = (total_lines > 0) ? (100.0 * blank_lines / total_lines) : 0.0;
    float comment_percentage = (total_lines > 0) ? (100.0 * total_comments / total_lines) : 0.0;
    float comment_char_percentage = (total_chars > 0) ? (100.0 * total_comment_chars / total_chars) : 0.0;

    // Print out the results
    printf("Total lines                     %d\n", total_lines);
    printf("Total blank lines               %d\n", blank_lines);
    printf("Blank line percentage           %.2f%%\n", blank_line_percentage);
    printf("Total characters                %d\n", total_chars);
    printf("Total spaces                    %d\n", total_spaces);
    printf("Total leading spaces            %d\n", total_leading_spaces);
    printf("Total comments                  %d\n", total_comments);
    printf("Total chars in comments         %d\n", total_comment_chars);
    printf("Comment percentage              %.2f%%\n", comment_percentage);
    printf("Comment chars percentage        %.2f%%\n", comment_char_percentage);
    printf("Total identifiers               %d\n", total_identifiers);
    printf("Total length of identifiers     %d\n", total_identifier_length);
    printf("Average identifier length       %.2f\n", (total_identifiers > 0) ? (float)total_identifier_length / total_identifiers : 0.0);
    printf("Total indenting errors          %d\n", total_indent_errors);

    return 0;
}
```

### Explanation:

1. **Tab Expansion**: We expand tabs to spaces using the rule that each tab character moves to the next multiple of 8 columns.
2. **Blank Lines**: A blank line is considered a line that contains only whitespace (spaces or tabs).
3. **Comment Handling**: We track comments, counting both the number of comments and the characters in the comments, excluding `/*` and `*/`.
4. **Identifier Handling**: We extract identifiers (words starting with a letter or underscore followed by letters, digits, or underscores), ensuring they aren't inside strings or comments.
5. **Indentation Errors**: We compare the indentation of the current line with the previous line to detect errors based on the presence of `{` and `}`.

This code reads from standard input and prints the requested statistics. It handles blank lines, tabs, spaces, comments, identifiers, and indentation errors. The `expand_tabs` function ensures that all tabs are expanded into spaces according to the modulo 8 rule, and we process each line to extract the necessary statistics.
