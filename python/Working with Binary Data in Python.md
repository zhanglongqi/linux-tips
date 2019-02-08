# Working with Binary Data in Python

Source: https://www.devdungeon.com/content/working-binary-data-python

## Overview

* Bytes Basics
  * Video: Bytes and Bytearray tutorial for Python 3
  * [The Bytes Type](#the-bytes-type)
  * [The Bytearray Type](#the-bytearray-type)
  * [The BytesIO Class](#the-bytesio-class)
* Working with files
  * [Writing Bytes to a File](#writing-bytes-to-a-file)
  * [Reading Bytes From a File](#reading-bytes-from-a-file)
  * [Read file line by line](#read-file-line-by-line)
  * [Getting the size of a file](#getting-the-size-of-a-file)
  * [Seeking a specific position in a file](#seeking-a-specific-position-in-a-file)
* Conversion, encoding, and output
  * [Integer to Bytes](#integer-to-bytes)
  * [Bytes to Integer](#bytes-to-integer)
  * [Text Encoding](#text-encoding)
  * [Base 64 Encoding](#base-64-encoding)
  * [Hexadecimal](#hexadecimal)
  * [Format Strings](#format-strings)
* Misc
  * [Bitwise Operations](#bitwise-operations)
  * [Struct Packing and Unpacking](#struct-packing-and-unpacking)
  * [System Byte Order](#system-byte-order)
* [Examples](#examples)
  * `diff.py` - Do two files match?
  * `is_jpeg.py` - Does the file have a JPEG binary signature?
  * `read_boot_sector.py` - Inspect the first 512 bytes of a file
  * `find_ascii_in_binary.py` - Identify ASCII characters in binary files
  * `create_stego_zip_jpg.py` - Hide a ZIP archive in a JPEG
  * `extract_pngs.py` - Extract PNGs from a file and store them in a pngs/ directory.

**All examples are in Python 3 and many will not work in Python 2.**

[Video: Bytes and Bytearray tutorial for Python 3](https://www.youtube.com/watch?time_continue=33&v=qnKX1y7HAyE)

## The Bytes Type

The bytes type in Python is immutable and stores a sequence of values ranging from 0-255 (8-bits). You can get the value of a single byte by using an index like an array, but the values can not be modified.

``` python
#Create empty bytes
empty_bytes = bytes(4)
print(type(empty_bytes))
print(empty_bytes)
```

## The Bytearray Type

To create a mutable object you need to use the bytearray type. With a bytearray you can do everything you can with other mutables like push, pop, insert, append, delete, and sort.

``` python
# Cast bytes to bytearray
mutable_bytes = bytearray(b'\x00\x0F')

# Bytearray allows modification
mutable_bytes[0] = 255
mutable_bytes.append(255)
print(mutable_bytes)

# Cast bytearray back to bytes
immutable_bytes = bytes(mutable_bytes)
print(immutable_bytes)
```

## The BytesIO Class

The io.BytesIO inherits from io.BufferedReader class comes with functions like read(), write(), peek(), getvalue(). It is a general buffer of bytes that you can work with.

``` python
binary_stream = io.BytesIO()
# Binary data and strings are different types, so a str
# must be encoded to binary using ascii, utf-8, or other.
binary_stream.write("Hello, world!\n".encode('ascii'))
binary_stream.write("Hello, world!\n".encode('utf-8'))

# Move cursor back to the beginning of the buffer
binary_stream.seek(0)

# Read all data from the buffer
stream_data = binary_stream.read()

# The stream_data is type 'bytes', immutable
print(type(stream_data))
print(stream_data)

# To modify the actual contents of the existing buffer
# use getbuffer() to get an object you can modify.
# Modifying this object updates the underlying BytesIO buffer
mutable_buffer = binary_stream.getbuffer()
print(type(mutable_buffer))  # class 'memoryview'
mutable_buffer[0] = 0xFF

# Re-read the original stream. Contents will be modified
# because we modified the mutable buffer
binary_stream.seek(0)
print(binary_stream.read())
```

## Writing Bytes to a File

``` python
# Pass "wb" to write a new file, or "ab" to append
with open("test.txt", "wb") as binary_file:
    # Write text or bytes to the file
    binary_file.write("Write text by encoding\n".encode('utf8'))
    num_bytes_written = binary_file.write(b'\xDE\xAD\xBE\xEF')
    print("Wrote %d bytes." % num_bytes_written)
```

Alternatively, you could explicitly call open and close, but if you do it this way you will need to do the error handling yourself and ensure the file is always closed, even if there is an error during writing. I don't recommend this method unless you have a strong reason.

``` python
binary_file = open("test.txt", "wb")
binary_file.write(b'\x00')
binary_file.close()
```

## Reading Bytes From a File

``` python
with open("test_file.dat", "rb") as binary_file:
    # Read the whole file at once
    data = binary_file.read()
    print(data)
```

## Read file line by line

If you are working a text file, you can read the data in line by line.

``` python
with open("test.txt", "rb") as text_file:
    # One option is to call readline() explicitly
    # single_line = text_file.readline()

    # It is easier to use a for loop to iterate each line
    for line in text_file:
        print(line)
```

## Getting the size of a file

``` python
import os
file_length_in_bytes = os.path.getsize("test.txt")
print(file_length_in_bytes)
```

## Seeking a specific position in a file

You can move to a specific position in file before reading or writing using seek(). You can pass a single parameter to seek() and it will move to that position, relative to the beginning of the file.

``` python

# Seek can be called one of two ways:
#   x.seek(offset)
#   x.seek(offset, starting_point)

# starting_point can be 0, 1, or 2
# 0 - Default. Offset relative to beginning of file
# 1 - Start from the current position in the file
# 2 - Start from the end of a file (will require a negative offset)

with open("test_file.dat", "rb") as binary_file:
    # Seek a specific position in the file and read N bytes
    binary_file.seek(0, 0)  # Go to beginning of the file
    couple_bytes = binary_file.read(2)
    print(couple_bytes)
```

## Integer to Bytes

``` python
i = 16

# Create one byte from the integer 16
single_byte = i.to_bytes(1, byteorder='big', signed=True) 
print(single_byte)

# Create four bytes from the integer
four_bytes = i.to_bytes(4, byteorder='big', signed=True)
print(four_bytes)

# Compare the difference to little endian
print(i.to_bytes(4, byteorder='little', signed=True))

# Create bytes from a list of integers with values from 0-255
bytes_from_list = bytes([255, 254, 253, 252])
print(bytes_from_list)

# Create a byte from a base 2 integer
one_byte = int('11110000', 2)
print(one_byte)

# Print out binary string (e.g. 0b010010)
print(bin(22))
```

## Bytes to Integer

``` python
# Create an int from bytes. Default is unsigned.
some_bytes = b'\x00\xF0'
i = int.from_bytes(some_bytes, byteorder='big')
print(i)

# Create a signed int
i = int.from_bytes(b'\x00\x0F', byteorder='big', signed=True)
print(i)

# Use a list of integers 0-255 as a source of byte values
i = int.from_bytes([255, 0, 0, 0], byteorder='big')
print(i)
```

## Text Encoding

``` python
# Binary to Text
binary_data = b'I am text.'
text = binary_data.decode('utf-8')
print(text)

binary_data = bytes([65, 66, 67])  # ASCII values for A, B, C
text = binary_data.decode('utf-8')
print(text)
```

``` python

# Text to Binary
message = "Hello"  # str
binary_message = message.encode('utf-8')
print(type(binary_message))  # bytes

# Python has many built in encodings for different languages,
# and even the Caeser cipher is built in
import codecs
cipher_text = codecs.encode(message, 'rot_13')
print(cipher_text)
```

## Base 64 Encoding

``` python
# Encode binary data to a base 64 string
binary_data = b'\x00\xFF\x00\xFF'

# Use the codecs module to encode
import codecs
base64_data = codecs.encode(binary_data, 'base64')
print(base64_data)

# Or use the binascii module
import binascii
base64_data = binascii.b2a_base64(binary_data)
print(base64_data)

# The base64_string is still a bytes type
# It may need to be decoded to an ASCII string
print(base64_data.decode('utf-8'))

# Decoding is done similarly
print(codecs.decode(base64_data, 'base64'))
print(binascii.a2b_base64(base64_data))
```

## Hexadecimal

``` python
# Starting with a hex string you can unhexlify it to bytes
deadbeef = binascii.unhexlify('DEADBEEF')
print(deadbeef)

# Given raw bytes, get an ASCII string representing the hex values
hex_data = binascii.hexlify(b'\x00\xff')  # Two bytes values 0 and 255

# The resulting value will be an ASCII string but it will be a bytes type
# It may be necessary to decode it to a regular string
text_string = hex_data.decode('utf-8')  # Result is string "00ff"
print(text_string)
```

## Format Strings

Format strings can be helpful to visualize or output byte values. Format strings require an integer value so the byte will have to be converted to an integer first.

``` python

a_byte = b'\xff'  # 255
i = ord(a_byte)   # Get the integer value of the byte

bin = "{0:b}".format(i) # binary: 11111111
hex = "{0:x}".format(i) # hexadecimal: ff
oct = "{0:o}".format(i) # octal: 377

print(bin)
print(hex)
print(oct)
```

## Bitwise Operations

``` python
# Some bytes to play with
byte1 = int('11110000', 2)  # 240
byte2 = int('00001111', 2)  # 15
byte3 = int('01010101', 2)  # 85

# Ones Complement (Flip the bits)
print(~byte1)

# AND
print(byte1 & byte2)

# OR
print(byte1 | byte2)

# XOR
print(byte1 ^ byte3)

# Shifting right will lose the right-most bit
print(byte2 >> 3)

# Shifting left will add a 0 bit on the right side
print(byte2 << 1)

# See if a single bit is set
bit_mask = int('00000001', 2)  # Bit 1
print(bit_mask & byte1)  # Is bit set in byte1?
print(bit_mask & byte2)  # Is bit set in byte2?
```

## Struct Packing and Unpacking

Packing and unpacking requires a string that defines how the binary data is structured. It needs to know which bytes represent values. It needs to know whether the entire set of bytes represets characters or if it is a sequence of 4-byte integers. It can be structured in any number of ways. The format strings can be simple or complex. In this example I am packing a single four-byte integer followed by two characters. The letters i and c represent integers and characters.

``` python

import struct

# Packing values to bytes
# The first parameter is the format string. Here it specifies the data is structured
# with a single four-byte integer followed by two characters.
# The rest of the parameters are the values for each item in order
binary_data = struct.pack("icc", 8499000, b'A', b'Z')
print(binary_data)

# When unpacking, you receive a tuple of all data in the same order
tuple_of_data = struct.unpack("icc", binary_data)
print(tuple_of_data)

# For more information on format strings and endiannes, refer to
# https://docs.python.org/3.5/library/struct.html
```

## System Byte Order

You might need to know what byte order your system uses. Byte order refers to big endian or little endian. The sys module can provide that value.

``` python
# Find out what byte order your system uses
import sys
print("Native byteorder: ", sys.byteorder)
```

## Examples

``` python
 # diff.py - Do two files match?
# Exercise: Rewrite this code to compare the files part at a time so it
# will not run out of RAM with large files.
import sys

with open(sys.argv[1], 'rb') as file1, open(sys.argv[2], 'rb') as file2:
    data1 = file1.read()
    data2 = file2.read()

if data1 != data2:
    print("Files do not match.")
else:
    print("Files match.")
 #is_jpeg.py - Does the file have a JPEG binary signature?

import sys
import binascii

jpeg_signatures = [
    binascii.unhexlify(b'FFD8FFD8'),
    binascii.unhexlify(b'FFD8FFE0'),
    binascii.unhexlify(b'FFD8FFE1')
]

with open(sys.argv[1], 'rb') as file:
    first_four_bytes = file.read(4)

    if first_four_bytes in jpeg_signatures:
        print("JPEG detected.")
    else:
        print("File does not look like a JPEG.")
 # read_boot_sector.py - Inspect the first 512 bytes of a file

import sys

in_file = open(sys.argv[1], 'rb')  # Provide a path to disk or ISO image
chunk_size = 512
data = in_file.read(chunk_size)
print(data)
 # find_ascii_in_binary.py - Identify ASCII characters in binary files

import sys
from functools import partial

chunk_size = 1
with open(sys.argv[1], 'rb') as in_file:    
    for data in iter(partial(in_file.read, chunk_size), b''):
        x = int.from_bytes(data, byteorder='big')
        if (x > 64 and x < 91) or (x > 96 and x < 123) :
            sys.stdout.write(chr(x))
        else:
            sys.stdout.write('.')
 # create_stego_zip_jpg.py - Hide a zip file inside a JPEG

import sys

# Start with a jpeg file
jpg_file = open(sys.argv[1], 'rb')  # Path to JPEG
jpg_data = jpg_file.read()
jpg_file.close()

# And the zip file to embed in the jpeg
zip_file = open(sys.argv[2], 'rb')  # Path to ZIP file
zip_data = zip_file.read()
zip_file.close()

# Combine the files
out_file = open('special_image.jpg', 'wb')  # Output file
out_file.write(jpg_data)
out_file.write(zip_data)
out_file.close()

# The resulting output file appears like a normal jpeg but can also be
# unzipped and used as an archive.

```

``` python
# extract_pngs.py
# Extract PNGs from a file and put them in a pngs/ directory

import sys
with open(sys.argv[1], "rb") as binary_file:
    binary_file.seek(0, 2)  # Seek the end
    num_bytes = binary_file.tell()  # Get the file size

    count = 0
    for i in range(num_bytes):
        binary_file.seek(i)
        eight_bytes = binary_file.read(8)
        if eight_bytes == b"\x89\x50\x4e\x47\x0d\x0a\x1a\x0a":  # PNG signature
            count += 1
            print("Found PNG Signature #" + str(count) + " at " + str(i))

            # Next four bytes after signature is the IHDR with the length
            png_size_bytes = binary_file.read(4)
            png_size = int.from_bytes(png_size_bytes, byteorder='little', signed=False)

            # Go back to beginning of image file and extract full thing
            binary_file.seek(i)
            # Read the size of image plus the signature
            png_data = binary_file.read(png_size + 8)
            with open("pngs/" + str(i) + ".png", "wb") as outfile:
                outfile.write(png_data)
```