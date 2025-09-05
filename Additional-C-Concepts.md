# Additional C Concepts

## Table of Contents
- [File I/O](#file-io)
- [Error Handling](#error-handling)
- [Command Line Arguments](#command-line-arguments)
- [Preprocessor Directives](#preprocessor-directives)
- [Bit Manipulation](#bit-manipulation)
- [Multi-threading](#multi-threading)
- [Networking](#networking)
- [Standard Library Functions](#standard-library-functions)

## File I/O
```c
#include <stdio.h>
#include <stdlib.h>

// File I/O Operations - Reading and writing files
void demonstrateFileIO() {
    // Writing to a file
    FILE* writeFile = fopen("example.txt", "w");  // Open for writing
    if (writeFile == NULL) {
        perror("Error opening file for writing");
        return;
    }

    fprintf(writeFile, "Hello, World!\n");        // Write formatted text
    fprintf(writeFile, "Number: %d\n", 42);      // Write with formatting
    fputs("Another line\n", writeFile);          // Write string
    fclose(writeFile);                           // Always close files

    // Reading from a file
    FILE* readFile = fopen("example.txt", "r");  // Open for reading
    if (readFile == NULL) {
        perror("Error opening file for reading");
        return;
    }

    char buffer[100];
    while (fgets(buffer, sizeof(buffer), readFile) != NULL) {  // Read line by line
        printf("Read: %s", buffer);
    }
    fclose(readFile);

    // Binary file operations
    int numbers[] = {1, 2, 3, 4, 5};
    FILE* binFile = fopen("data.bin", "wb");     // Open for binary writing
    fwrite(numbers, sizeof(int), 5, binFile);   // Write binary data
    fclose(binFile);

    // Reading binary data
    int readNumbers[5];
    binFile = fopen("data.bin", "rb");           // Open for binary reading
    fread(readNumbers, sizeof(int), 5, binFile); // Read binary data
    fclose(binFile);
}

// File positioning
void demonstrateFilePositioning() {
    FILE* file = fopen("example.txt", "r+");     // Open for read/write
    if (file == NULL) return;

    fseek(file, 0, SEEK_END);                    // Move to end of file
    long fileSize = ftell(file);                 // Get current position (file size)
    printf("File size: %ld bytes\n", fileSize);

    fseek(file, 0, SEEK_SET);                    // Move to beginning
    rewind(file);                                // Alternative way to go to beginning

    fclose(file);
}
```

## Error Handling
```c
#include <errno.h>
#include <string.h>

// Error Handling - Using errno and perror
void demonstrateErrorHandling() {
    FILE* file = fopen("nonexistent.txt", "r");
    if (file == NULL) {
        printf("Error number: %d\n", errno);         // Print error number
        perror("Error opening file");               // Print error description
        printf("Error message: %s\n", strerror(errno)); // Alternative error message
    }

    // Custom error handling
    int result = someFunction();
    if (result == -1) {
        fprintf(stderr, "Custom error occurred\n");  // Write to stderr
        exit(EXIT_FAILURE);                          // Exit with failure code
    }
}

// Custom error codes
#define SUCCESS 0
#define ERROR_FILE_NOT_FOUND 1
#define ERROR_INVALID_INPUT 2

typedef struct {
    int code;
    const char* message;
} ErrorInfo;

const char* getErrorMessage(int errorCode) {
    static ErrorInfo errors[] = {
        {SUCCESS, "Operation successful"},
        {ERROR_FILE_NOT_FOUND, "File not found"},
        {ERROR_INVALID_INPUT, "Invalid input provided"}
    };

    for (size_t i = 0; i < sizeof(errors)/sizeof(errors[0]); i++) {
        if (errors[i].code == errorCode) {
            return errors[i].message;
        }
    }
    return "Unknown error";
}
```

## Command Line Arguments
```c
#include <stdio.h>

// Command Line Arguments - Processing program arguments
int main(int argc, char* argv[]) {
    printf("Program name: %s\n", argv[0]);        // First argument is program name
    printf("Number of arguments: %d\n", argc - 1); // argc includes program name

    // Process each argument
    for (int i = 1; i < argc; i++) {
        printf("Argument %d: %s\n", i, argv[i]);
    }

    // Example: Simple argument parsing
    if (argc > 1) {
        if (strcmp(argv[1], "--help") == 0 || strcmp(argv[1], "-h") == 0) {
            printf("Usage: %s [options] [filename]\n", argv[0]);
            printf("Options:\n");
            printf("  -h, --help    Show this help message\n");
            printf("  -v, --version Show version information\n");
            return 0;
        } else if (strcmp(argv[1], "--version") == 0 || strcmp(argv[1], "-v") == 0) {
            printf("Version 1.0.0\n");
            return 0;
        }
    }

    return 0;
}

// Advanced argument parsing example
typedef struct {
    char* inputFile;
    char* outputFile;
    int verbose;
    int help;
} ProgramOptions;

ProgramOptions parseArguments(int argc, char* argv[]) {
    ProgramOptions options = {NULL, NULL, 0, 0};

    for (int i = 1; i < argc; i++) {
        if (strcmp(argv[i], "-i") == 0 && i + 1 < argc) {
            options.inputFile = argv[++i];
        } else if (strcmp(argv[i], "-o") == 0 && i + 1 < argc) {
            options.outputFile = argv[++i];
        } else if (strcmp(argv[i], "-v") == 0) {
            options.verbose = 1;
        } else if (strcmp(argv[i], "--help") == 0) {
            options.help = 1;
        }
    }

    return options;
}
```

## Preprocessor Directives
```c
#include <stdio.h>
#include <stdlib.h>

// Preprocessor Directives - Conditional compilation and macros

// Simple macro definition
#define PI 3.14159
#define SQUARE(x) ((x) * (x))
#define MAX(a, b) ((a) > (b) ? (a) : (b))

// Conditional compilation
#define DEBUG 1
#define PLATFORM_WINDOWS 1

void demonstratePreprocessor() {
    double radius = 5.0;
    double area = PI * SQUARE(radius);  // Using macros
    printf("Area: %f\n", area);

    // Conditional compilation based on debug flag
    #ifdef DEBUG
        printf("Debug mode enabled\n");
    #else
        printf("Release mode\n");
    #endif

    // Platform-specific code
    #ifdef PLATFORM_WINDOWS
        printf("Running on Windows\n");
        system("cls");  // Windows clear screen
    #else
        printf("Running on other platform\n");
        system("clear");  // Unix clear screen
    #endif
}

// Include guards example
#ifndef MY_HEADER_H
#define MY_HEADER_H

// Header content goes here
void myFunction();

#endif // MY_HEADER_H

// Macro with variable arguments
#define LOG(format, ...) printf("[LOG] " format "\n", ##__VA_ARGS__)
#define ERROR(format, ...) fprintf(stderr, "[ERROR] " format "\n", ##__VA_ARGS__)

void demonstrateLogging() {
    LOG("Program started");
    LOG("Value: %d", 42);
    ERROR("Something went wrong: %s", "file not found");
}
```

## Bit Manipulation
```c
#include <stdio.h>

// Bit Manipulation - Advanced bitwise operations
void demonstrateBitManipulation() {
    unsigned int flags = 0;

    // Setting bits (flags)
    flags |= (1 << 0);  // Set bit 0 (enable feature 1)
    flags |= (1 << 2);  // Set bit 2 (enable feature 3)
    flags |= (1 << 5);  // Set bit 5 (enable feature 6)

    // Checking bits
    if (flags & (1 << 0)) {  // Check if bit 0 is set
        printf("Feature 1 is enabled\n");
    }

    // Clearing bits
    flags &= ~(1 << 2);  // Clear bit 2 (disable feature 3)

    // Toggling bits
    flags ^= (1 << 1);  // Toggle bit 1

    // Bit counting
    int count = 0;
    unsigned int temp = flags;
    while (temp) {
        count += temp & 1;  // Count set bits
        temp >>= 1;
    }
    printf("Number of set bits: %d\n", count);

    // Bit masking
    unsigned int mask = 0x0F;  // Mask for lower 4 bits
    unsigned int lowerNibble = flags & mask;
    unsigned int upperNibble = (flags >> 4) & mask;

    printf("Lower nibble: 0x%X\n", lowerNibble);
    printf("Upper nibble: 0x%X\n", upperNibble);
}

// Bit fields for compact storage
typedef struct {
    unsigned int feature1 : 1;  // 1 bit
    unsigned int feature2 : 1;  // 1 bit
    unsigned int priority : 3;  // 3 bits (0-7)
    unsigned int reserved : 27; // Remaining bits
} SystemFlags;

void demonstrateBitFields() {
    SystemFlags flags = {0};

    flags.feature1 = 1;     // Enable feature 1
    flags.priority = 5;     // Set priority to 5

    printf("Feature 1: %d\n", flags.feature1);
    printf("Priority: %d\n", flags.priority);
    printf("Size of struct: %zu bytes\n", sizeof(SystemFlags));
}
```

## Multi-threading
```c
#include <windows.h>  // Windows threading
#include <stdio.h>

// Multi-threading - Windows threads example
DWORD WINAPI threadFunction(LPVOID param) {
    int threadId = *(int*)param;
    printf("Thread %d started\n", threadId);

    // Simulate work
    Sleep(1000);  // Sleep for 1 second

    printf("Thread %d finished\n", threadId);
    return 0;
}

void demonstrateThreading() {
    const int NUM_THREADS = 3;
    HANDLE threads[NUM_THREADS];
    int threadIds[NUM_THREADS];

    // Create threads
    for (int i = 0; i < NUM_THREADS; i++) {
        threadIds[i] = i + 1;
        threads[i] = CreateThread(
            NULL,           // Default security attributes
            0,              // Default stack size
            threadFunction, // Thread function
            &threadIds[i],  // Parameter to thread function
            0,              // Default creation flags
            NULL            // Thread ID (optional)
        );

        if (threads[i] == NULL) {
            printf("Error creating thread %d\n", i + 1);
            return;
        }
    }

    // Wait for all threads to complete
    WaitForMultipleObjects(NUM_THREADS, threads, TRUE, INFINITE);

    // Close thread handles
    for (int i = 0; i < NUM_THREADS; i++) {
        CloseHandle(threads[i]);
    }

    printf("All threads completed\n");
}

// Thread synchronization with mutex
HANDLE mutex;

void demonstrateMutex() {
    mutex = CreateMutex(NULL, FALSE, NULL);

    // In thread function, use:
    // WaitForSingleObject(mutex, INFINITE);  // Lock
    // ... critical section ...
    // ReleaseMutex(mutex);                   // Unlock

    CloseHandle(mutex);
}
```

## Networking
```c
#include <winsock2.h>  // Windows sockets
#include <ws2tcpip.h>
#include <stdio.h>
#pragma comment(lib, "ws2_32.lib")  // Link with ws2_32.lib

// Networking - Basic TCP client example
void demonstrateNetworking() {
    WSADATA wsaData;
    if (WSAStartup(MAKEWORD(2, 2), &wsaData) != 0) {
        printf("WSAStartup failed\n");
        return;
    }

    // Create socket
    SOCKET sock = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);
    if (sock == INVALID_SOCKET) {
        printf("Socket creation failed\n");
        WSACleanup();
        return;
    }

    // Connect to server
    struct sockaddr_in serverAddr;
    serverAddr.sin_family = AF_INET;
    serverAddr.sin_port = htons(80);  // HTTP port
    inet_pton(AF_INET, "127.0.0.1", &serverAddr.sin_addr);

    if (connect(sock, (struct sockaddr*)&serverAddr, sizeof(serverAddr)) == SOCKET_ERROR) {
        printf("Connection failed\n");
        closesocket(sock);
        WSACleanup();
        return;
    }

    // Send HTTP request
    const char* request = "GET / HTTP/1.1\r\nHost: localhost\r\n\r\n";
    send(sock, request, strlen(request), 0);

    // Receive response
    char buffer[1024];
    int bytesReceived = recv(sock, buffer, sizeof(buffer) - 1, 0);
    if (bytesReceived > 0) {
        buffer[bytesReceived] = '\0';
        printf("Received: %s\n", buffer);
    }

    // Cleanup
    closesocket(sock);
    WSACleanup();
}

// Simple TCP server example
void demonstrateServer() {
    WSADATA wsaData;
    WSAStartup(MAKEWORD(2, 2), &wsaData);

    SOCKET serverSock = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);

    struct sockaddr_in serverAddr;
    serverAddr.sin_family = AF_INET;
    serverAddr.sin_addr.s_addr = INADDR_ANY;
    serverAddr.sin_port = htons(8080);

    bind(serverSock, (struct sockaddr*)&serverAddr, sizeof(serverAddr));
    listen(serverSock, SOMAXCONN);

    printf("Server listening on port 8080...\n");

    // Accept client connection
    SOCKET clientSock = accept(serverSock, NULL, NULL);
    if (clientSock != INVALID_SOCKET) {
        char buffer[1024];
        int bytesReceived = recv(clientSock, buffer, sizeof(buffer), 0);
        if (bytesReceived > 0) {
            buffer[bytesReceived] = '\0';
            printf("Client sent: %s\n", buffer);

            // Send response
            const char* response = "Hello from server!";
            send(clientSock, response, strlen(response), 0);
        }
        closesocket(clientSock);
    }

    closesocket(serverSock);
    WSACleanup();
}
```

## Standard Library Functions
```c
#include <stdlib.h>
#include <string.h>
#include <math.h>
#include <time.h>
#include <ctype.h>

// Standard Library Functions - More utilities from stdlib, string.h, math.h
void demonstrateStdLib() {
    // Memory operations
    int* array = (int*)calloc(10, sizeof(int));  // Allocate and zero memory
    memset(array, 0, 10 * sizeof(int));         // Set memory to zero
    memcpy(array, (int[]){1,2,3,4,5}, 5 * sizeof(int)); // Copy memory

    // String operations
    char str[] = "Hello, World!";
    char* found = strchr(str, ',');             // Find character
    char* token = strtok(str, " ,!");           // Tokenize string

    // Mathematical functions
    double angle = M_PI / 4;                     // Pi constant
    double sine = sin(angle);                    // Sine function
    double cosine = cos(angle);                  // Cosine function
    double squareRoot = sqrt(16.0);              // Square root
    double power = pow(2.0, 3.0);                // Power function
    double rounded = round(3.7);                 // Rounding

    printf("sin(π/4) = %f\n", sine);
    printf("cos(π/4) = %f\n", cosine);
    printf("√16 = %f\n", squareRoot);
    printf("2³ = %f\n", power);
    printf("round(3.7) = %f\n", rounded);

    free(array);
}

void demonstrateTime() {
    time_t currentTime = time(NULL);            // Get current time
    struct tm* localTime = localtime(&currentTime);

    printf("Current date and time: %s", asctime(localTime));

    // Time formatting
    char timeString[100];
    strftime(timeString, sizeof(timeString), "%Y-%m-%d %H:%M:%S", localTime);
    printf("Formatted time: %s\n", timeString);
}

void demonstrateCharacterFunctions() {
    char ch = 'a';

    // Character classification
    if (isalpha(ch)) printf("%c is alphabetic\n", ch);
    if (islower(ch)) printf("%c is lowercase\n", ch);
    if (isdigit('5')) printf("5 is a digit\n");

    // Character conversion
    char upper = toupper(ch);                   // Convert to uppercase
    char lower = tolower('B');                  // Convert to lowercase

    printf("Uppercase: %c\n", upper);
    printf("Lowercase: %c\n", lower);
}

void demonstrateRandom() {
    srand(time(NULL));                          // Seed random number generator

    // Generate random numbers
    int randomInt = rand() % 100;               // Random int 0-99
    double randomDouble = (double)rand() / RAND_MAX; // Random double 0.0-1.0

    printf("Random int: %d\n", randomInt);
    printf("Random double: %f\n", randomDouble);
}
