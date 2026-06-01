# 42 School — Milestone 0

Welcome to **Milestone 0** of the 42 School curriculum! This is where every 42 student begins their journey. Milestone 0 focuses on building a **custom C library (libft)** that will serve as the foundation for all future projects.

## Overview

Milestone 0 consists of a single, comprehensive project: **libft** — your own implementation of essential C standard library functions, plus bonus features.

| Project | Type | Language | Key Concepts |
|---------|------|----------|--------------|
| **libft** | Foundation Library | C | String manipulation, memory management, data structures, function pointers |

---

## Project: Libft

### What It Is

**Libft** (short for "library 42") is a C library where you reimplement common standard library functions (`libc`) from scratch, without using them. This teaches you:
- How standard library functions actually work internally
- Memory management and pointer arithmetic
- String and buffer manipulation
- Data structure design (linked lists)
- Best practices in C programming

The libft library becomes your personal toolkit—you'll link it to nearly every project in the 42 curriculum.

### Part 1: Mandatory Functions

These are reimplementations of standard C library functions:

#### Character Classification (is* functions)
- `ft_isalpha()`: Check if character is alphabetic (a-z, A-Z)
- `ft_isdigit()`: Check if character is a digit (0-9)
- `ft_isalnum()`: Check if alphanumeric (letter or digit)
- `ft_isascii()`: Check if ASCII character (0-127)
- `ft_isprint()`: Check if printable character

#### Memory Functions
- `ft_memset()`: Fill memory with constant byte
- `ft_bzero()`: Clear memory area (fill with zeros)
- `ft_memcpy()`: Copy memory area (non-overlapping)
- `ft_memmove()`: Copy memory area (handles overlapping)
- `ft_memchr()`: Locate byte in memory
- `ft_memcmp()`: Compare memory areas

#### String Functions - Basic
- `ft_strlen()`: Calculate string length
- `ft_strchr()`: Find first occurrence of character in string
- `ft_strrchr()`: Find last occurrence of character in string
- `ft_strncmp()`: Compare strings (up to n characters)
- `ft_strnstr()`: Find substring in string (safe version)

#### String Functions - Concatenation & Copying
- `ft_strlcpy()`: Copy string with size limit (prevents overflow)
- `ft_strlcat()`: Concatenate strings with size limit

#### Case Conversion
- `ft_toupper()`: Convert character to uppercase
- `ft_tolower()`: Convert character to lowercase

#### Number Conversion
- `ft_atoi()`: ASCII to integer (convert string "123" to 123)
- `ft_itoa()`: Integer to ASCII (convert 123 to "123" string) [bonus-like]

#### Memory Allocation
- `ft_calloc()`: Allocate and zero-initialize memory
- `ft_strdup()`: Duplicate string (allocate and copy)

#### Advanced String Manipulation
- `ft_substr()`: Extract substring from string
- `ft_strjoin()`: Concatenate two strings with allocation
- `ft_strtrim()`: Remove leading/trailing characters
- `ft_split()`: Split string by delimiter into array

#### Output Functions
- `ft_putchar_fd()`: Write character to file descriptor
- `ft_putstr_fd()`: Write string to file descriptor
- `ft_putendl_fd()`: Write string + newline to file descriptor
- `ft_putnbr_fd()`: Write number to file descriptor

#### String Iteration (Function Pointers)
- `ft_strmapi()`: Apply function to each character, create new string
- `ft_striteri()`: Apply function to each character and index (in-place)

#### Additional Utilities
- `ft_uitoa()`: Unsigned integer to ASCII (bonus-like)

### Part 2: Bonus Functions (Linked Lists)

These functions work with a custom linked list data structure:

```c
typedef struct s_list
{
    void            *content;
    struct s_list   *next;
} t_list;
```

#### Linked List Operations
- `ft_lstnew()`: Create new list node with content
- `ft_lstadd_front()`: Add node to beginning of list
- `ft_lstadd_back()`: Add node to end of list
- `ft_lstsize()`: Count number of nodes in list
- `ft_lstlast()`: Get last node of list
- `ft_lstdelone()`: Delete single node and free its content
- `ft_lstclear()`: Delete entire list and free all content
- `ft_lstiter()`: Apply function to every node's content
- `ft_lstmap()`: Create new list by applying function to each node

### Why These Functions Matter

Learning to implement these teaches:

1. **Pointer Arithmetic**: Understanding memory layout and manipulation
2. **Buffer Management**: Handling fixed-size buffers safely
3. **Edge Cases**: Empty strings, NULL pointers, boundary conditions
4. **Function Pointers**: Passing functions as parameters for flexibility
5. **Data Structures**: Linked lists and generic node design

### Example Function: ft_strlen

What's trivial in standard C:
```c
#include <string.h>
int len = strlen("hello");  // Result: 5
```

What you must implement:
```c
size_t ft_strlen(const char *s)
{
    size_t i;
    
    i = 0;
    while (s[i])
        i++;
    return (i);
}
```

Seems simple, but contains important lessons:
- Iterate through string until null terminator
- Use `size_t` for length (unsigned type)
- Handle edge case: empty string (just null terminator)

### Example Function: ft_split (More Complex)

Split "hello,world,42" by comma into ["hello", "world", "42", NULL]:

```c
char **ft_split(char const *s, char c)
{
    // 1. Count how many substrings exist
    // 2. Allocate array large enough + 1 for NULL terminator
    // 3. For each substring:
    //    - Find end delimiter
    //    - Allocate memory for that substring
    //    - Copy substring
    // 4. Add NULL terminator to array
    // 5. Return array
}
```

More challenging aspects:
- Dynamic memory allocation
- String extraction and copying
- Proper cleanup on errors
- Memory leak prevention

### Compilation & Usage

```bash
cd libft
make               # Compile mandatory functions
make bonus         # Compile with bonus (linked lists)
make clean         # Remove .o files
make fclean        # Remove .o files and library
make re            # Recompile everything

# After compilation, creates: libft.a
# Link it: gcc main.c -L. -lft -o program
```

### The Makefile

A critical part of libft is learning to write an effective Makefile:
- Compile multiple source files into object files
- Create static library (`.a`) from object files
- Use variables for compiler flags, sources, objects
- Implement clean, fclean, re, bonus targets

### Common Pitfalls

1. **Off-by-one errors**: Incorrect loop boundaries in string functions
2. **NULL pointer handling**: Not checking for NULL inputs
3. **Buffer overflow**: Not respecting size limits in copy/cat functions
4. **Memory leaks**: Forgetting to free allocated memory
5. **Return values**: Not returning correct types (size_t vs int)

---

## The 42 Approach: Norminette

At 42, code quality isn't optional. Your code must pass **Norminette**, an automated style checker:

- Lines max 80 characters
- Functions max 25 lines
- Maximum 5 local variables per function
- No more than 2 function levels of nesting
- Strict formatting rules

This teaches discipline and forces clean, readable code.

---

## Learning Outcomes

### C Fundamentals
- Pointers and indirection
- Memory layout and addressing
- String representation in C (null-terminated arrays)
- Type casting and implicit conversions

### Software Engineering
- Writing reusable library code
- Documentation through comments
- Function design and modularity
- Build automation with Makefiles

### Debugging & Optimization
- Using `gdb` to debug pointer issues
- Understanding memory through `valgrind`
- Profiling for optimization
- Writing test programs to verify functionality

---

## Architecture & Design Patterns

### Defensive Programming
Each function should validate inputs:
```c
// Safe: Check for NULL
if (!s || !*s)
    return (0);  // or default value
```

### Consistent Return Types
- Functions modifying data: return modified pointer or count
- Predicate functions: return 1 for true, 0 for false
- Allocation functions: return pointer or NULL on failure

### Function Pointers for Flexibility
```c
// Apply any function to each character
char *ft_strmapi(char const *s, char (*f)(unsigned int, char))
{
    // ... 
    result[i] = f(i, s[i]);  // Call the passed function
    // ...
}
```

This pattern enables callbacks and higher-order functions in C.

---

## Curriculum Path After Libft

Every subsequent 42 project builds on libft:

| Milestone | Projects | Uses libft |
|-----------|---------|-----------|
| **0** | **libft** | **Foundation** |
| 1 | ft_printf, get_next_line | Uses extensively |
| 2 | push_swap, minitalk, fract-ol | Uses extensively |
| 3 | minishell, philosophers | Uses extensively |
| 4 | cub3d | Uses extensively |

You'll link `-lft` to every project for years.

---

## Expected Code Statistics

- **Lines of Code**: 1,500-3,000 (depending on implementation style)
- **Functions**: 42+ (including bonus)
- **Time Estimate**: 1-2 weeks for comfortable completion

---

## Testing & Validation

### Manual Testing Approach
```bash
# Create a test program
gcc -c ft_strlen.c
gcc -c ft_atoi.c
# ... compile all
ar rcs libft.a *.o

# Write test_main.c
gcc test_main.c -L. -lft -o test
./test
```

### Validation Checklist
- Handles empty input (empty string, empty list)
- Handles single element
- Handles large inputs
- Handles special characters
- Handles NULL pointers appropriately
- No memory leaks (verify with valgrind)
- Follows 42 Norminette standards

---

## Advanced Concepts to Understand

### Static Libraries
- What goes into `.a` files
- How linking combines object files
- Why separate compilation speeds up development

### Function Pointers
- Passing functions as parameters
- Callbacks and higher-order functions
- Generic code that works with any function

### Generic Data Structures
- The `t_list` structure uses `void *content`
- Allows storing any data type
- Trade-off: Loss of type safety

---

## Resources & References

### C Standard Library Documentation
- `man` pages: `man 3 strlen`, `man 3 strcpy`, etc.
- Open Group Base Specifications: https://pubs.opengroup.org/

### Learning Resources
- The C Programming Language (Kernighan & Ritchie)
- C Pointers documentation
- Memory management best practices

### Testing Tools
- `valgrind`: Memory leak detection
- `gdb`: Debugger for step-through debugging
- `nm`: List symbols in object files
- `ar`: Create and manipulate archives

---

## Tips for Success

1. **Start Strong**: Understand the simple functions first (strlen, atoi, toupper)

2. **Build Incrementally**: Complete mandatory before attempting bonus

3. **Test Heavily**: Write test programs for each function
   ```bash
   gcc -c src/*.c
   ar rcs libft.a *.o
   gcc test.c -L. -lft -o test
   ./test
   ```

4. **Handle Edge Cases**: Empty strings, NULL pointers, boundary values

5. **Use Valgrind**: Check for memory leaks
   ```bash
   valgrind --leak-check=full ./test
   ```

6. **Read Man Pages**: Understanding behavior of real functions helps
   ```bash
   man 3 malloc
   man 3 strlen
   ```

7. **Code Cleanly**: Follow norminette early, not at the end

8. **Bonus Last**: Get mandatory working perfectly first

---

## Checklist for Completion

### Mandatory Part
- [ ] All character classification functions implemented
- [ ] All memory functions implemented (memset, memcpy, etc.)
- [ ] All string functions implemented
- [ ] Number conversion functions working (atoi, itoa)
- [ ] Dynamic allocation functions work safely
- [ ] Advanced string functions (split, strmapi, striteri) work
- [ ] File descriptor output functions implemented
- [ ] Code passes norminette check
- [ ] No memory leaks (verified with valgrind)
- [ ] Makefile creates static library correctly

### Bonus Part
- [ ] Linked list structure defined
- [ ] All list functions implemented (new, add_front, add_back)
- [ ] List manipulation functions work (size, last, clear)
- [ ] Higher-order functions work (iter, map)
- [ ] No memory leaks in list functions

---

**Libft is the foundation of your 42 journey. Get it right—you'll use it thousands of times over the next years.**

Good luck, and welcome to 42!
