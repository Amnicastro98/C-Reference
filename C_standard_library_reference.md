# C Standard Library Quick Reference

This document lists popular functions provided by common C standard library headers.

---

##  `stdio.h` (Standard Input/Output)
Functions for input, output, and file handling.

- **`printf(const char *format, ...)`** → Prints formatted output to `stdout`.  
- **`scanf(const char *format, ...)`** → Reads formatted input from `stdin`.  
- **`putchar(int c)`** → Writes a single character to `stdout`.  
- **`getchar(void)`** → Reads a single character from `stdin`.  
- **`puts(const char *str)`** → Writes a string followed by a newline to `stdout`.  
- **`gets(char *str)`** → (**unsafe**, removed in C11) Reads a line from `stdin`.  
- **`fopen(const char *filename, const char *mode)`** → Opens a file stream.  
- **`fclose(FILE *stream)`** → Closes a file stream.  
- **`fgets(char *str, int n, FILE *stream)`** → Reads a string from a file (safe version of `gets`).  
- **`fputs(const char *str, FILE *stream)`** → Writes a string to a file.  
- **`fread(void *ptr, size_t size, size_t nmemb, FILE *stream)`** → Reads binary data from a file.  
- **`fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream)`** → Writes binary data to a file.  
- **`fprintf(FILE *stream, const char *format, ...)`** → Prints formatted output to a file.  
- **`fscanf(FILE *stream, const char *format, ...)`** → Reads formatted input from a file.  
- **`feof(FILE *stream)`** → Tests if the end of a file has been reached.  
- **`ferror(FILE *stream)`** → Tests if an error has occurred in a file stream.  

---

## `stdlib.h` (Standard Library Utilities)
Functions for memory, conversion, random numbers, process control.

- **`malloc(size_t size)`** → Allocates a block of memory.  
- **`calloc(size_t nmemb, size_t size)`** → Allocates and zeroes a block of memory.  
- **`realloc(void *ptr, size_t size)`** → Resizes a block of memory.  
- **`free(void *ptr)`** → Frees previously allocated memory.  
- **`exit(int status)`** → Terminates the program with a status code.  
- **`abort(void)`** → Terminates the program abnormally.  
- **`system(const char *command)`** → Executes a shell command.  
- **`atexit(void (*func)(void))`** → Registers a function to run on program exit.  
- **`rand(void)`** → Returns a pseudo-random number.  
- **`srand(unsigned int seed)`** → Seeds the random number generator.  
- **`abs(int x)`** → Absolute value of an integer.  
- **`labs(long x)`** → Absolute value of a long.  
- **`atoi(const char *str)`** → Converts a string to an `int`.  
- **`atof(const char *str)`** → Converts a string to a `double`.  
- **`atol(const char *str)`** → Converts a string to a `long`.  
- **`strtol(const char *str, char **endptr, int base)`** → Converts a string to a `long` with error checking.  
- **`strtod(const char *str, char **endptr)`** → Converts a string to a `double` with error checking.  
- **`qsort(void *base, size_t nitems, size_t size, int (*compar)(const void*, const void*))`** → Sorts an array.  
- **`bsearch(const void *key, const void *base, size_t nitems, size_t size, int (*compar)(const void*, const void*))`** → Binary search in a sorted array.  

---

## `string.h` (String and Memory Handling)
Functions for manipulating strings and memory blocks.

- **`strlen(const char *str)`** → Returns length of a string.  
- **`strcpy(char *dest, const char *src)`** → Copies one string to another.  
- **`strncpy(char *dest, const char *src, size_t n)`** → Copies up to `n` characters.  
- **`strcat(char *dest, const char *src)`** → Concatenates two strings.  
- **`strncat(char *dest, const char *src, size_t n)`** → Concatenates up to `n` characters.  
- **`strcmp(const char *s1, const char *s2)`** → Compares two strings.  
- **`strncmp(const char *s1, const char *s2, size_t n)`** → Compares up to `n` characters.  
- **`strchr(const char *str, int c)`** → Finds the first occurrence of a character.  
- **`strrchr(const char *str, int c)`** → Finds the last occurrence of a character.  
- **`strstr(const char *haystack, const char *needle)`** → Finds the first occurrence of a substring.  
- **`strtok(char *str, const char *delim)`** → Tokenizes a string (splits by delimiters).  
- **`memcpy(void *dest, const void *src, size_t n)`** → Copies memory block.  
- **`memmove(void *dest, const void *src, size_t n)`** → Copies memory (safe for overlapping regions).  
- **`memcmp(const void *s1, const void *s2, size_t n)`** → Compares memory blocks.  
- **`memset(void *str, int c, size_t n)`** → Sets memory to a value.  

---

## `stdbool.h` (Boolean Type)
Introduced in **C99** to give C a real boolean type.

- **`bool`** → Alias for `_Bool` (values are `0` or `1`).  
- **`true`** → Boolean constant equal to `1`.  
- **`false`** → Boolean constant equal to `0`.  

(No functions here — just definitions for booleans.)
