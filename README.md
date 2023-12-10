# crystr | Compile-Time Strings and Numbers Encryption for C++20

## Description
This repository provides a compile-time strings and numbers encryption mechanism based on xor bitwise operations using mathematical operations at compile-time in order to make the strings and numbers challenging to decompile.\
Users can easily encrypt their strings using the `crystr` macro provided in the header, and numbers using `crynum` and `crynum_long`.\
The repository includes an example demonstrating the usage of `crystr`, `crynum` and `crynum_long` to use encrypted strings and numbers.\
It's recommended, but not necessarily, to `disable the optimization under C/C++ -> Optimization -> "Optimization" and "Whole Program Optimization"`\
When using the virtual functions option it `needs RTTI OFF (Run-Time Type Information under C/C++ -> Language (/GR-))`

## Key Aspects
 - The xor key is generated by mathematical operations based on a uniqueid that is different every time the program is compiled and for each encrypted string/number.
 - The uniqueid is an hash based on the date, time and a counter which increases every time a string/number encrypts: ```const_hash(__DATE__ __TIME__) + __COUNTER__ * __COUNTER__```.
 - When using crystr every character is encrypted with a different xor key based on the index of the character inside the string.
 - It is possible to choose between virtual and inline functions to decrypt the strings/numbers.
 - If using the virtual implementation it works well with `crycall` ([see the repo](https://github.com/Android1337/crycall)).
 - All the variables used to retrieve the xor key are thread_local, meaning they're stored as `(NtCurrentTeb()->ThreadLocalStoragePointer + TlsIndex)->x_var`, making it more challenging to decompile and **not** ready-pastable from decompiler tools such as ida or ghidra.
 - The string is never copied but instead it's being written on, in order to prevent unwanted decrypted copies of the string in memory.
 - The decrypted string can be re-encrypted at any time using the built-in `encrypt` function.
 - The decrypted/encrypted string/number can be removed from memory at any time using the built-in `clear` function.
 - The string/number will show as never referenced in most common decompiler tools such as ida or ghidra see ([How it shows](https://github.com/Android1337/crystr/tree/main#how-it-shows)).
 - Compatible with char[], wchar_t[], int, __int64, unsigned int, unsigned __int64, float, double.
 - Supports C++20 and higher versions.

## How it shows
[Look here](https://imgur.com/a/acamGoW)\
How it shows using [inline functions option](https://crystr-inline.tiiny.site)

## Repository Structure
- **`include/`**: Contains the `crystr.hpp` header file providing the compile-time string/number encryption mechanism.
- **`src/`**: Holds the example `main.cpp` file showcasing the usage of `crystr`, `crynum`, `crynum_long`.
- **`LICENSE`**: Licensing information for the provided code.
- **`README.md`**: Documentation explaining how to use everything.

## Usage Example
The repository includes an example demonstrating the usage of the `crystr`, `crynum`, `crynum_long` macros:

### `main.cpp`
```cpp
//#include "crycall.hpp" // see https://github.com/Android1337/crycall for an all-potential virtual implementation
#include "crystr.hpp"

int main() {
    auto encrypted_str = crystr("Hello, this is an encrypted string!");
    printf("Decrypted String: %s\n", encrypted_str.decrypt());
    encrypted_str.clear();

    auto encrypted_int = crynum(1234);
    printf("Decrypted Int: %d\n", encrypted_int.decrypt());
    encrypted_int.clear();

    auto encrypted_double = crynum_long(1.234);
    printf("Decrypted Double: %f\n", encrypted_double.decrypt());
    encrypted_double.clear();

    return 0;
}
```
