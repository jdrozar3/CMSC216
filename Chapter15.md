# Chapter 15: Input/Output Functions

## Error Reporting

- whenever an error occurs, C library functions save an error code in `errno` (an `extern` variable defined in `errno.h`)
- `perror(char const *message);` prints `message` followed by `: ` and then the message associated with the `errno` code
   - message must be non-NULL and point to a valid string
- `void exit(int status);` terminates program execution with the status code of `status`

## I/O Concepts

### Streams

- abstraction for I/O device interaction presented as a sequence of data concerned with input or output
- *text stream*: sequence of characters terminated by a newline (on UNIX machines)
   - on systems with differing definitions of text, library functions translate between representations
   - for example, Windows writes newlines as a pair of carriage return/newline characters
- *binary stream*: sequence of bytes
   - binary data is NOT modified by library functions (this guarantee does not exist with text data)

### Buffers

- area of memory data is copied into before being *flushed* or written to final destination (increases I/O speed)
- types:
  - *unbuffered*: data is written directly to final destination without buffer
  - *line buffering*: buffer is flushed when newline character is read
  - *block buffering*: buffer is flushed in blocks of arbitrary size

### FILEs

- data structure giving a program access to a stream
- runtime environment must give access to at least three streams: standard input, standard output, standard error
  - `stdin`: pointer to `FILE` giving access to standard input
  - `stdout`: pointer to `FILE` giving access to standard output
  - `stderr`: pointer to `FILE` giving access to standard error

## Stream I/O

- declare a `FILE *` for each file that must be simultaneously opened
- call `fopen()` and set your `FILE *` above to the return value
- process the file
- dall `fclose()` on the `FILE *` to release it for reuse with another file

```C
    FILE * fopen(char const *name, char const *mode);
    
    // opens a file and associates a stream with it (saving return value links the FILE pointed to by the variable with this stream)
    // return value is NULL if the operation fails
    
    // name: name of the file
    // mode: 'r', 'w', 'a' (read, write, append) for text files or 'rb', 'wb', 'ab' (read, write, append) for binary files
    
    // example
    FILE *input;
    input = fopen("data.txt", 'r');
    if (input == NULL) {
        perror("data.txt");
        exit(EXIT_FAILURE);
    }
````