# C Elements Quick Reference

## Table of Contents
- [Variables and Data Types](#variables-and-data-types)
- [Operators](#operators)
- [Control Structures](#control-structures)
- [Arrays](#arrays)
- [Functions](#functions)
- [String Operations](#string-operations)

## Variables and Data Types
```c
// Primitive Data Types
int number = 42;                  // Integer values
double decimal = 3.14;            // Decimal numbers
_Bool flag = 1;                   // 1 for true, 0 for false (use _Bool or int)
char letter = 'A';                // Single character
short mediumNumber = 32767;       // 16-bit integer
long bigNumber = 123456789L;      // Long integer
float floatNumber = 3.14f;        // Single precision

// Type Modifiers
unsigned int positive = 100;      // Only positive values
signed int withSign = -100;       // Positive and negative
long long veryBig = 1234567890LL; // Extra large numbers

// Constants
const double PI = 3.14159;        // Constant value
const char* NAME = "C";           // Constant string

// No auto type deduction in C

// Pointers (no references in C)
int* ptr = &number;               // Pointer
```

## Operators
```c
// Arithmetic Operators
int sum = 5 + 3;                 // Addition
int difference = 10 - 4;         // Subtraction
int product = 4 * 2;             // Multiplication
int quotient = 15 / 3;           // Division
int remainder = 7 % 3;           // Modulus

// Compound Assignment
int number = 10;
number += 5;                     // Add and assign
number -= 3;                     // Subtract and assign
number *= 2;                     // Multiply and assign
number /= 4;                     // Divide and assign

// Comparison Operators
_Bool isEqual = (5 == 5);         // Equal to
_Bool notEqual = (5 != 3);        // Not equal to
_Bool greater = (7 > 3);          // Greater than
_Bool less = (4 < 8);             // Less than
_Bool greaterEqual = (5 >= 5);    // Greater than or equal
_Bool lessEqual = (4 <= 4);       // Less than or equal

// Logical Operators
_Bool and_result = 1 && 0;        // Logical AND
_Bool or_result = 1 || 0;         // Logical OR
_Bool not_result = !1;            // Logical NOT

// Bitwise Operators
int bitAnd = 5 & 3;             // Bitwise AND
int bitOr = 5 | 3;              // Bitwise OR
int bitXor = 5 ^ 3;             // Bitwise XOR
int leftShift = 5 << 1;         // Left shift
int rightShift = 5 >> 1;        // Right shift

// Increment/Decrement
int count = 0;
count++;                         // Post-increment
++count;                         // Pre-increment
count--;                         // Post-decrement
--count;                         // Pre-decrement
```

## Control Structures
```c
// If-Else Statements
if (score >= 90) {
    printf("A grade\n");
} else if (score >= 80) {
    printf("B grade\n");
} else {
    printf("Below B\n");
}

// Switch Statement
switch (day) {
    case 1:                      // Case with break
        printf("Monday\n");
        break;
    case 2:                      // Fall-through case
        printf("Tuesday\n");
    default:
        printf("Other day\n");
}

// While Loop
int i = 0;
while (i < 5) {                 // Loop with condition
    printf("%d\n", i);
    i++;
}

// Do-While Loop
do {                           // Executes at least once
    printf("%d\n", i);
    i--;
} while (i > 0);

// For Loop
for (int j = 0; j < 5; j++) {  // Standard for loop
    printf("%d\n", j);
}

// No range-based for loop in C
```

## Arrays
```c
// Array Declaration and Initialization
int numbers[5];                 // Fixed-size array
int values[] = {1, 2, 3, 4, 5}; // Array with initializer
const char* names[3];           // Array of strings

// Multi-dimensional Arrays
int matrix[3][3];              // 2D array
int grid[][3] = {              // 2D array with initializer
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// Array Operations
numbers[0] = 10;               // Setting element
int value = numbers[0];        // Getting element
size_t length = sizeof(numbers) / sizeof(numbers[0]); // Array length

// No modern array alternatives like std::array or std::vector in C
```

## Functions
```c
// Basic Function
void printMessage() {
    printf("Hello!\n");
}

// Function with Parameters
int add(int a, int b) {
    return a + b;
}

// Function with Multiple Returns (use pointers or structs)
char* getGrade(int score) {
    if (score >= 90) return "A";
    if (score >= 80) return "B";
    return "C";
}

// No function overloading in C

// No default parameters in C

// Inline functions not standard in C (use macros or compiler extensions)
#define SQUARE(x) ((x) * (x))
```

## String Operations
```c
#include <string.h>

// String Creation
char str1[] = "Hello";          // C-style string array
const char* str2 = "Hi";        // C-style string pointer

// String Methods
char text[] = "Hello World";
size_t length = strlen(text);   // String length
char first = text[0];           // Get character
// Substring: use memcpy or custom function
char sub[6];
memcpy(sub, text, 5);
sub[5] = '\0';

// String Comparison
_Bool equals = (strcmp(str1, "Hello") == 0); // Content comparison
int compare = strcmp(str1, str2);    // Compare strings

// String Manipulation
char concat[20];
strcpy(concat, str1);
strcat(concat, " ");
strcat(concat, str2);           // Concatenation
// Replace: use custom logic
// Append: use strcat
strcat(text, " There");         // Append
// Insert: use custom logic

// No string stream in standard C; use sprintf
char buffer[50];
sprintf(buffer, "Number: %d", 42);
```
