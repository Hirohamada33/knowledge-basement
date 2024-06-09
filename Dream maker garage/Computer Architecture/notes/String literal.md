The `char` type in C is for storage of character data. It has the size of a byte on the target architecture. 
(Note that this means it may not be sufficient to hold an entire character of a particular language – in some languages, multiple chars or larger types may be required to store characters)

##### Encoding
The encoding of a char is platform dependent. 
In the vast majority of cases, characters 0-127 are defined through the **American Standard Code for Information Interchange** (**ASCII**), with some slight variations dependent on locale (Alternatives to **ASCII** include encodings such as **EBCDIC**, mostly just seen on **IBM Z-machines** nowadays)
In ASCII, characters 0-31 and 127 are control characters, while 32-126 are printable characters. 
Characters 128-255 (or higher, in the case of platforms with non-octet bytes) **vary greatly** and **cannot be relied upon** unless information about the locale is known. In some cases, these are simply an **extended set of characters** (e.g. Latin-1). In other cases, characters in this range indicate a **multibyte encoding** (e.g. UTF-8, Shift-JIS)

##### Literals
Escape sequences starting with a backslash `\` can be used to represent
characters that cannot otherwise be represented. Some examples:
`char z = '\0' // zero byte`
`char n = '\n' // new line`
`char r = '\r' // carriage return`
`char q = '\'' // single quote`

Escape sequences consisting of an octal or hexadecimal number can be used to refer to characters directly by their value:
`char octal = '\30'`
`char hex = '\x6F'`
The character literals are actually `int` type, so it could mean it is unnecessary to use escape literals, but directly an `int` type. But these escapes are still useful when used in string literals. 


##### Strings
Strings are sequences of `chars`. It is not an explicit type, but actually an array of characters, terminated with a `NUL`character (`\0`)
In function passing, the first character of the string is passed by a pointer (`(const) char * ptr`). 
The `NUL` terminator is implicit added automatically, it allows functions to know the end of the string. For instance, `printf(ptr)`; will print the character at `ptr`, then the next character, and so on until the `NUL` terminator is encountered. 
Escape sequences can be used in both character literal and string literals. 