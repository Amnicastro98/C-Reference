# C Basics Quick Reference

## Table of Contents
- [Structs and Functions](#structs-and-functions)
- [Composition](#composition)
- [Function Pointers](#function-pointers)
- [Abstract Interfaces](#abstract-interfaces)
- [Polymorphism](#polymorphism)
- [Encapsulation](#encapsulation)

## Structs and Functions
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <stdbool.h>

// Basic Struct Structure
typedef struct {
    char brand[50];           // Member variables
    int year;
    bool isRunning;
} Car;

// Constructor function
Car* Car_create(const char* b, int y) {
    Car* car = (Car*)malloc(sizeof(Car));
    if (car) {
        strcpy(car->brand, b);
        car->year = y;
        car->isRunning = false;
    }
    return car;
}

// Member function (method)
void Car_startEngine(Car* this) {
    this->isRunning = true;
    printf("Engine started!\n");
}

// Getter method
const char* Car_getBrand(const Car* this) {
    return this->brand;
}

// Setter method
void Car_setYear(Car* this, int y) {
    if (y > 1900) {          // Data validation
        this->year = y;
    }
}

// Destructor function
void Car_destroy(Car* this) {
    printf("Car object destroyed\n");
    free(this);
}

// Creating and Using Structs
Car* myCar = Car_create("Toyota", 2020);  // Create object
Car_startEngine(myCar);                   // Call method
const char* brand = Car_getBrand(myCar);  // Use getter
Car_destroy(myCar);                       // Cleanup
```

## Composition
```c
// Base Struct
typedef struct {
    char name[50];
} Animal;

// Constructor
Animal* Animal_create(const char* n) {
    Animal* animal = (Animal*)malloc(sizeof(Animal));
    if (animal) {
        strcpy(animal->name, n);
    }
    return animal;
}

// Virtual function equivalent (function pointer)
typedef void (*MakeSoundFunc)(const void*);

// Derived Struct with composition
typedef struct {
    Animal base;             // Composition instead of inheritance
    char breed[50];
    MakeSoundFunc makeSound; // Function pointer for polymorphism
} Dog;

// Constructor
Dog* Dog_create(const char* n, const char* b) {
    Dog* dog = (Dog*)malloc(sizeof(Dog));
    if (dog) {
        strcpy(dog->base.name, n);
        strcpy(dog->breed, b);
        dog->makeSound = Dog_makeSound; // Assign function
    }
    return dog;
}

// Override base method
void Dog_makeSound(const void* this) {
    const Dog* dog = (const Dog*)this;
    printf("Woof!\n");
}

// Dog-specific method
void Dog_fetch(const Dog* this) {
    printf("%s is fetching!\n", this->base.name);
}

// Using Composition
Dog* myDog = Dog_create("Rex", "Labrador");
myDog->makeSound(myDog);     // Calls Dog's version
Dog_fetch(myDog);            // Dog-specific method
free(myDog);                 // Cleanup
```

## Function Pointers
```c
// Function Pointer Example
typedef double (*AreaFunc)(const void*);

typedef struct {
    AreaFunc area;
} Shape;

// Circle implementation
typedef struct {
    Shape base;
    double radius;
} Circle;

// Constructor
Circle* Circle_create(double r) {
    Circle* circle = (Circle*)malloc(sizeof(Circle));
    if (circle) {
        circle->base.area = Circle_area;
        circle->radius = r;
    }
    return circle;
}

// Implement area function
double Circle_area(const void* this) {
    const Circle* circle = (const Circle*)this;
    return 3.14159 * circle->radius * circle->radius;
}

// Usage
Circle* circle = Circle_create(5);
double area = circle->base.area(circle); // Polymorphic call
free(circle);
```

## Abstract Interfaces
```c
// Abstract Interface (struct with function pointers)
typedef struct {
    void (*draw)(void* self);
    void (*resize)(void* self, int size);
} Drawable;

// Concrete implementation
typedef struct {
    Drawable base;
    int width, height;
} Rectangle;

// Constructor
Rectangle* Rectangle_create(int w, int h) {
    Rectangle* rect = (Rectangle*)malloc(sizeof(Rectangle));
    if (rect) {
        rect->base.draw = Rectangle_draw;
        rect->base.resize = Rectangle_resize;
        rect->width = w;
        rect->height = h;
    }
    return rect;
}

// Implement interface functions
void Rectangle_draw(void* self) {
    printf("Drawing rectangle\n");
}

void Rectangle_resize(void* self, int size) {
    Rectangle* rect = (Rectangle*)self;
    rect->width *= size;
    rect->height *= size;
}

// Multiple interfaces (composition)
typedef struct {
    Shape shape;
    Drawable drawable;
    // Must implement all functions
} Square;
```

## Polymorphism
```c
// Polymorphic Struct Hierarchy
typedef void (*MoveFunc)(void*);

typedef struct {
    MoveFunc move;
} Vehicle;

// Car implementation
typedef struct {
    Vehicle base;
} CarStruct;

// Constructor
CarStruct* CarStruct_create() {
    CarStruct* car = (CarStruct*)malloc(sizeof(CarStruct));
    if (car) {
        car->base.move = CarStruct_move;
    }
    return car;
}

void CarStruct_move(void* self) {
    printf("Car driving\n");
}

// Boat implementation
typedef struct {
    Vehicle base;
} Boat;

// Constructor
Boat* Boat_create() {
    Boat* boat = (Boat*)malloc(sizeof(Boat));
    if (boat) {
        boat->base.move = Boat_move;
    }
    return boat;
}

void Boat_move(void* self) {
    printf("Boat sailing\n");
}

// Polymorphic Usage
void startJourney(Vehicle* vehicle) {
    vehicle->move(vehicle);  // Calls appropriate version
}

int main() {
    CarStruct* car = CarStruct_create();
    Boat* boat = Boat_create();
    
    startJourney((Vehicle*)car);  // "Car driving"
    startJourney((Vehicle*)boat); // "Boat sailing"
    
    free(car);
    free(boat);
    return 0;
}
```

## Encapsulation
```c
// Encapsulated Struct (convention: don't access members directly)
typedef struct {
    char accountNumber[20];
    double balance;
    char ownerName[50];
} BankAccount;

// Constructor
BankAccount* BankAccount_create(const char* accNum, const char* owner) {
    BankAccount* account = (BankAccount*)malloc(sizeof(BankAccount));
    if (account) {
        strcpy(account->accountNumber, accNum);
        account->balance = 0.0;
        strcpy(account->ownerName, owner);
    }
    return account;
}

// Const member function equivalent
double BankAccount_getBalance(const BankAccount* this) {
    return this->balance;
}

// Member functions with error handling
void BankAccount_deposit(BankAccount* this, double amount) {
    if (amount > 0) {
        this->balance += amount;
        printf("Deposit successful\n");
    }
}

bool BankAccount_withdraw(BankAccount* this, double amount) {
    if (amount > 0 && amount <= this->balance) {
        this->balance -= amount;
        printf("Withdrawal successful\n");
        return true;
    }
    return false;
}

// Copy function (manual copy control)
BankAccount* BankAccount_copy(const BankAccount* other) {
    return BankAccount_create(other->accountNumber, other->ownerName);
}

// Assignment function
void BankAccount_assign(BankAccount* this, const BankAccount* other) {
    if (this != other) {
        strcpy(this->accountNumber, other->accountNumber);
        this->balance = other->balance;
        strcpy(this->ownerName, other->ownerName);
    }
}

// Using Encapsulated Struct
BankAccount* account = BankAccount_create("123456", "John Doe");
BankAccount_deposit(account, 1000);     // Safe transaction
double balance = BankAccount_getBalance(account); // Safe access
BankAccount_destroy(account);           // Cleanup
```
