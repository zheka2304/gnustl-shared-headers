# Standart C++ Headers for gnustl_shared.so

This headers are used by Inner Core and Inner Core native mods to link gnustl_shared.so and minecraft library, based on it. This headers can be used along with standard headers without conflict, as they are using separate namespace.

## Installation
When using toolchanin, create directory `toolchain/stdincludes/gnu_stl` and copy `stl` directory from this repo into it. You can then use it like this `#include <stl/vector.h>`.

You can also copy stl directory to your sources root, but in this case you will need to include it as header from your sources.

## Usage
Namespace `std::__ndk1` declared in headers is identical to `std` namespace, so if you need to use for example vector from this namespace, you include `<stl/vector.h>` and use `std::__ndk1::vector<int>`. 

But consider, that types, declared in `std::__ndk1` differ in internal structure from ones in `std` namespace and cannot be replaced by their counterpart.

## Common practices

Never use `using namespace std::__ndk1` to reduce amount of code, this is a bad practice even in case of one standard namespace, but in this case, when you have 2 standard namespaces, it will become a terrible mess.

Consider using global macro instead, like so:
```
// std::__ndk1::string -> STL::string
#define STL std::__ndk1
```

String conversion can be easily done via `data()` method:
```
std::string s = "a";
STL::string stl_s = s.data();
std::string std_s = stl_s.data();
```

