# Milestone 0 — libft

Milestone 0 at 42 Luxembourg is **libft**: a static C library containing my versions of common libc functions, some extra string utilities, and a small linked-list API. The source is in [`libft/`](libft/). I use functions from this library in later C projects rather than rewriting the same helpers each time.

The point of the exercise is to understand what functions such as `memmove`, `strlcat`, and `calloc` actually do: how far they read or write, who owns allocated memory, what they return, and what happens at the boundaries.

## Functions

The implementations use an `ft_` prefix. The complete declarations are in [`libft/libft.h`](libft/libft.h).

### Character tests and conversion

| Function | Behavior |
| --- | --- |
| `ft_isalpha` | Checks for an ASCII letter. |
| `ft_isdigit` | Checks for a decimal digit. |
| `ft_isalnum` | Checks for a letter or digit. |
| `ft_isascii` | Checks for a value in the ASCII range. |
| `ft_isprint` | Checks for a printable ASCII character. |
| `ft_toupper`, `ft_tolower` | Convert ASCII letter case; leave other characters unchanged. |

### Memory

| Function | Behavior |
| --- | --- |
| `ft_memset` | Writes a byte value into a memory region. |
| `ft_bzero` | Fills a memory region with zero bytes. |
| `ft_memcpy` | Copies `n` bytes between non-overlapping regions. |
| `ft_memmove` | Copies `n` bytes safely even if source and destination overlap. |
| `ft_memchr` | Looks for a byte in the first `n` bytes of a region. |
| `ft_memcmp` | Compares the first `n` bytes of two regions. |
| `ft_calloc` | Allocates zero-initialized space for an array. |

`memcpy` and `memmove` aren't interchangeable. If the destination overlaps the source, copying forward may overwrite bytes that haven't been read yet. `memmove` chooses a safe copy direction. `calloc` also has to account for the size of an allocation, not just set the result to zero.

### Strings

| Function | Behavior |
| --- | --- |
| `ft_strlen` | Counts characters up to the terminating `\0`. |
| `ft_strchr`, `ft_strrchr` | Find the first/last occurrence of a character. |
| `ft_strncmp` | Compare up to `n` characters. |
| `ft_strnstr` | Look for a substring within a bounded prefix. |
| `ft_strlcpy`, `ft_strlcat` | Size-aware copying and concatenation. |
| `ft_strdup` | Allocate a copy of a string. |
| `ft_substr` | Allocate a substring. |
| `ft_strjoin` | Allocate the concatenation of two strings. |
| `ft_strtrim` | Trim specified characters from both ends. |
| `ft_split` | Split a string on a delimiter into a NULL-terminated array. |
| `ft_strmapi` | Build a new string by applying a callback to each character. |
| `ft_striteri` | Apply a callback to each character of an existing string. |

Functions returning newly allocated strings transfer ownership to the caller. For `ft_split`, that includes both the array and each of its elements.

### Numbers and output

| Function | Behavior |
| --- | --- |
| `ft_atoi` | Parse a signed integer from text. |
| `ft_itoa` | Allocate the decimal representation of an `int`. |
| `ft_uitoa` | Additional helper for converting an unsigned integer. |
| `ft_putchar_fd` | Write one character to a file descriptor. |
| `ft_putstr_fd` | Write a string to a file descriptor. |
| `ft_putendl_fd` | Write a string followed by a newline. |
| `ft_putnbr_fd` | Write an integer to a file descriptor. |

The `_fd` functions take a file descriptor rather than being fixed to standard output. That also makes them useful for writing errors to descriptor 2.

## Bonus: linked lists

The bonus functions operate on a singly linked list whose content can point to any type:

```c
typedef struct s_list
{
    void            *content;
    struct s_list   *next;
} t_list;
```

| Function | Behavior |
| --- | --- |
| `ft_lstnew` | Allocate a new node. |
| `ft_lstadd_front`, `ft_lstadd_back` | Insert a node at either end. |
| `ft_lstsize`, `ft_lstlast` | Count nodes or find the last one. |
| `ft_lstdelone` | Delete a node, applying the supplied content-deletion function. |
| `ft_lstclear` | Delete an entire list. |
| `ft_lstiter` | Call a function on each node's content. |
| `ft_lstmap` | Build a new list from a transformation of each node. |

Using `void *content` makes the list generic, but it also means the list cannot know how to free its contents. That's why `ft_lstdelone`, `ft_lstclear`, and `ft_lstmap` receive callbacks. In particular, `ft_lstmap` needs to release its partially created list if an allocation fails.

## Two examples

### Walking a string

`ft_strlen` reads until it reaches the null terminator. The terminator is not included in the returned length.

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

This is a small function, but the `size_t` return type, empty-string case, and `\0` termination are the basis of most of the other string functions.

### Splitting and cleaning up

`ft_split("hello,world,42", ',')` produces three separately allocated strings plus a final `NULL` pointer:

```text
result ──┬──> "hello"
         ├──> "world"
         ├──> "42"
         └──> NULL
```

The implementation has to count the words, allocate the pointer array, locate each word, and allocate its copy. Consecutive delimiters and delimiters at the ends must not generate unwanted empty entries. If any allocation fails, earlier strings and the array must be freed before returning `NULL`. Simply freeing the outer array would leak memory.

## Build and link

From the repository root:

```sh
cd libft
make             # mandatory functions -> libft.a
make bonus       # add linked-list functions to libft.a
make clean       # delete object files
make fclean      # delete object files and libft.a
make re          # full mandatory rebuild
```

The Makefile compiles each `.c` into an object using `cc -Wall -Wextra -Werror`, then `ar rcs` packages the objects into `libft.a`. A static library is an archive of object files, not a program you can run by itself.

To use the library from a test program located in `libft/`:

```c
#include "libft.h"

int main(void)
{
    ft_putendl_fd("hello from libft", 1);
    return (0);
}
```

```sh
cc -Wall -Wextra -Werror main.c -I. -L. -lft -o test_libft
./test_libft
```

The `-I` flag locates the header, while `-L.` and `-lft` tell the linker where to find `libft.a`.

## Things worth testing

- **Boundaries:** empty strings, zero-length operations, one-byte buffers, and strings without a matching character.
- **Copying:** `memmove` with source/destination overlap in both directions; `strlcpy` and `strlcat` with truncated destinations.
- **Allocations:** allocation failures and cleanup paths in `split`, `substr`, `strjoin`, and `lstmap`.
- **Conversions:** zero, negative values, and integer limits, including `INT_MIN` for `ft_itoa`.
- **Lists:** empty list, one-node list, adding at both ends, and clearing all allocated contents.

For memory errors, compile a small test executable and run `valgrind --leak-check=full ./test_libft`. `norminette` checks the 42 formatting rules; it does not check correctness. Manual tests are still needed, particularly for functions where one extra byte is enough to corrupt memory.

References: the relevant `man 3` pages, the [Open Group specifications](https://pubs.opengroup.org/), and *The C Programming Language* (Kernighan and Ritchie).
