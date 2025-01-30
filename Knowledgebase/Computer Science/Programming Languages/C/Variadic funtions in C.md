```c
#include <stdarg.h>
#include <stdio.h>

void printNumbers(int count, ...) {
    va_list args;       // Declare a va_list to handle the variable arguments
    va_start(args, count);  // Initialize the va_list with the last known parameter

    for (int i = 0; i < count; i++) {
        int number = va_arg(args, int);  // Access the next argument
        printf("%d ", number);
    }

    va_end(args);       // Clean up the va_list
    printf("\n");
}

int main() {
    printNumbers(3, 10, 20, 30);  // Outputs: 10 20 30
    return 0;
}
```
