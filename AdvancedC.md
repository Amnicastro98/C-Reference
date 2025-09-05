# C Advanced Concepts

## Table of Contents
- [Generic Macros](#generic-macros)
- [Manual Smart Pointer Management](#manual-smart-pointer-management)
- [Custom Containers](#custom-containers)
- [Function Pointers and Callbacks](#function-pointers-and-callbacks)
- [Move Semantics Emulation](#move-semantics-emulation)
- [Memory Management](#memory-management)

## Generic Macros
```c
#include <stdio.h>

// Generic max macro using _Generic (C11 feature for type selection)
#define max(a, b) _Generic((a), \
    int: max_int, \
    double: max_double \
)(a, b)  // Select function based on type

int max_int(int a, int b) {
    return (a > b) ? a : b;
}

double max_double(double a, double b) {
    return (a > b) ? a : b;
}

// Usage - compiler selects appropriate function
int x = max(3, 5);        // Calls max_int
double y = max(3.14, 2.71); // Calls max_double
```

## Manual Smart Pointer Management
```c
#include <stdlib.h>
#include <stdio.h>

// Reference counted pointer (manual shared_ptr equivalent)
typedef struct {
    int* ptr;       // The actual data
    int* ref_count; // Reference counter
} SharedPtr;

SharedPtr SharedPtr_create(int* p) {
    SharedPtr sp;
    sp.ptr = p;
    sp.ref_count = (int*)malloc(sizeof(int));
    *sp.ref_count = 1;  // Start with 1 reference
    return sp;
}

void SharedPtr_addRef(SharedPtr* sp) {
    (*sp->ref_count)++;  // Increment reference count
}

void SharedPtr_release(SharedPtr* sp) {
    (*sp->ref_count)--;  // Decrement reference count
    if (*sp->ref_count == 0) {
        free(sp->ptr);      // Free data when last reference
        free(sp->ref_count); // Free counter
        sp->ptr = NULL;
        sp->ref_count = NULL;
    }
}

// Usage - manual memory management
SharedPtr sp1 = SharedPtr_create(malloc(sizeof(int)));
*sp1.ptr = 42;           // Use the data
SharedPtr_addRef(&sp1);  // Add another reference
SharedPtr_release(&sp1); // Release first reference
SharedPtr_release(&sp1); // Release second - frees memory
```

## Custom Containers
```c
#include <stdlib.h>
#include <stdio.h>

typedef struct {
    int* data;      // Dynamic array
    size_t size;    // Current number of elements
    size_t capacity; // Maximum capacity
} Vector;

Vector* Vector_create(size_t capacity) {
    Vector* v = (Vector*)malloc(sizeof(Vector));
    v->data = (int*)malloc(sizeof(int) * capacity);
    v->size = 0;
    v->capacity = capacity;
    return v;
}

void Vector_push_back(Vector* v, int value) {
    if (v->size >= v->capacity) {
        v->capacity *= 2;  // Double capacity when full
        v->data = (int*)realloc(v->data, sizeof(int) * v->capacity);
    }
    v->data[v->size++] = value;  // Add element and increment size
}

void Vector_destroy(Vector* v) {
    free(v->data);  // Free array
    free(v);        // Free struct
}

// Usage - manual vector implementation
Vector* vec = Vector_create(4);
Vector_push_back(vec, 10);  // Add elements
Vector_push_back(vec, 20);
Vector_destroy(vec);        // Cleanup
```

## Function Pointers and Callbacks
```c
#include <stdio.h>

typedef void (*Callback)(int);  // Function pointer type

void register_callback(Callback cb) {
    cb(42);  // Call the function through pointer
}

void my_callback(int value) {
    printf("Callback called with value %d\n", value);
}

// Usage - passing function as parameter
register_callback(my_callback);  // Pass function pointer
```

## Move Semantics Emulation
```c
#include <stdlib.h>
#include <string.h>
#include <stdio.h>

typedef struct {
    int* data;
    size_t size;
} Buffer;

Buffer Buffer_create(size_t size) {
    Buffer buf;
    buf.data = (int*)malloc(sizeof(int) * size);
    buf.size = size;
    return buf;
}

void Buffer_move(Buffer* dest, Buffer* src) {
    dest->data = src->data;  // Transfer ownership
    dest->size = src->size;
    src->data = NULL;        // Null source pointers
    src->size = 0;
}

void Buffer_destroy(Buffer* buf) {
    if (buf->data) {
        free(buf->data);
        buf->data = NULL;
    }
    buf->size = 0;
}

// Usage - manual move semantics
Buffer createBuffer(size_t size) {
    return Buffer_create(size);
}

int main() {
    Buffer buf1 = createBuffer(1000);
    Buffer buf2;
    Buffer_move(&buf2, &buf1);  // Move ownership
    Buffer_destroy(&buf1);      // Safe to destroy (null pointers)
    Buffer_destroy(&buf2);
    return 0;
}
```

## Memory Management
```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

// Raw Memory Operations
void* raw = malloc(sizeof(int));
free(raw);

// Placement New equivalent: manual construction/destruction
typedef struct {
    char data[50];
} Buffer;

void Buffer_init(Buffer* buf, const char* str) {
    strcpy(buf->data, str);
}

void Buffer_destroy(Buffer* buf) {
    // No automatic destructor in C, manual cleanup if needed
}

// Custom Allocator example omitted for brevity

// Memory Pool example omitted for brevity

// RAII Pattern emulation using cleanup functions
typedef struct {
    int* resource;
} ResourceGuard;

void ResourceGuard_init(ResourceGuard* guard, int* res) {
    guard->resource = res;
}

void ResourceGuard_destroy(ResourceGuard* guard) {
    free(guard->resource);
}

// Usage
int* res = malloc(sizeof(int));
ResourceGuard guard;
ResourceGuard_init(&guard, res);
// Use resource
ResourceGuard_destroy(&guard);
```
