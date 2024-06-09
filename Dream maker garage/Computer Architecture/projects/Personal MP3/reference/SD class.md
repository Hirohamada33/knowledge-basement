##### SD.begin()
Initializes the SD library and card. This begins use of the **SPI bus** (digital pins 11, 12, and 13 on most Arduino boards; 50, 51, and 52 on the Mega) and the chip select pin, which defaults to the hardware SS pin (pin 10 on most Arduino boards, 53 on the Mega). Note that even if you use a different chip select pin, **the hardware SS pin must be kept as an output or the SD library functions will not work**.

**Syntax:**
```c
SD.begin(cspin)
```

**Parameters:**
`cspin`(optional parameter): the pin connected to the chip select line of the SD card; defaults to the hardware SS line of the SPI bus.

**Returns:**
1 on success, 0 on failure.


##### SD.exists()
Tests whether a file or directory exists on the SD card.

**Syntax:**
```c
SD.exist(filename)
```

**Parameters:**
`filename`: the name of the file to test for existence, which can include directories (delimited by forward-slashes, `/`).

**Returns:**
1 if the file exists, 0 if doesn't.

##### SD.mkdir()
Create a directory on the SD card. This will also create any intermediate directories that don’t already exists; e.g. `SD.mkdir("a/b/c")` will create a, b, and c.

**Syntax:**
```c
SD.mkdir(filename)
```

**Parameters:**
`filename`: the name of the file to test for existence, which can include directories (delimited by forward-slashes, `/`).

**Returns:**
1 if successful on creating the file, 0 if doesn't.


##### SD.open()
Opens a file on the SD card. If the file is opened for writing, it will be created if it doesn’t already exist (but the directory containing it must already exist).

**Syntax:**
```c
SD.open(filepath)
SD.open(filepath, mode)
```

**Parameters:**
`filepath`: the name of the file to open, which can include directories (delimited by forward-slashes, `/`).
`mode` (optional): the mode in which to open the file. Mode can be `FILE_READ` (open the file for reading, starting at the beginning of the file) or `FILE_WRITE` (open the file for reading and writing, starting at the end of the file).

**Returns:**
A **File object** referring to the opened file; if the file couldn’t be opened, this object will evaluate to false in a boolean context, i.e. it can be tested by the return value with “`if (f)`”.


##### SD.remove()
Remove a file from the SD card.

**Syntax:**
```c
SD.remove(filename)
```

**Parameters:**
`filename`: the name of the file to test for existence, which can include directories (delimited by forward-slashes, `/`).

**Returns:**
1 if the removal of the file succeeded, 0 if not.


##### SD.rmdir()
Remove a directory from the SD card. The directory must be empty.

**Syntax:**
```c
SD.rmdir(filename)
```

**Parameters:**
`filename`: the name of the file to test for existence, which can include directories (delimited by forward-slashes, `/`).

**Returns:**
1 if the removal of the directory succeeded, 0 if not (if the directory didn’t exist, the return value is unspecified).


##### SD.mkdir()
Create a directory on the SD card. This will also create any intermediate directories that don’t already exists; e.g. `SD.mkdir("a/b/c")` will create a, b, and c.

**Syntax:**
```c
SD.mkdir(filename)
```

**Parameters:**
`filename`: the name of the file to test for existence, which can include directories (delimited by forward-slashes, `/`).

**Returns:**
1 if successful on creating the file, 0 if doesn't.
