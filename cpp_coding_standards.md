# keen games C++ coding standards v0.3 (3.1.2019)

## Prime Directive
* __code has to be simple, fast and self-explanatory__
* No surprises!
* Make reading/understanding of code easier than writing

## C++ Features

### Features that are mandatory / encouraged:

* C++11: static assertions -> you *can* use the KEEN_STATIC_ASSERT macro instead
* C++11: nullptr -> use it everywhere / never use 0 for pointers
* C++11: C++ attributes: noreturn, deprecated, nodiscard
* C++11: constexpr values -> use for named constants (over enums/defines/const)
* C++11: explicit virtual overrides
* C++11: strongly typed enums (nearly always specify the underlying type)
* C++11: final specifier
* C++11: inline namespaces -> sure, why not.. maybe try it for versioning?
* C++11: non static data member initializers -> prefer a struct with member initializers over a class with constructor for 'public'/'parameter' structs
* C++14: binary literals

### 'Meh' Features which should be only used when they are really worth it in the situation

* C++11: variadic templates -> minimize use / only for formatString/scanString
* C++11: template aliasing
* C++11: decltype -> only useful in some template meta programming code (which should be minimized)
* C++11: constexpr functions -> not enough experience yet.. should be fine when used rarely
* C++11: delegating constructors -> sure, why not.. but minimizing the use of constructors is preferred
* C++11: user-defined literals -> only use it for Strings (__.""\_s".__)
* C++11: default functions -> sure, why not.. but avoid 'default' functions whenever possible
* C++11: deleted functions
* C++11: range based for loops -> *can* be used but often a normal / raw loop is easier
* C++11: explicit conversion functions (*if* you need to write a conversion operator it probably should be explicit)
* C++11: type_traits
* C++17: inline variables (prefer constexpr)
* C++17: nested namespaces
* C++17: constexpr if (better than alternatives, but try to minimize meta-programming)

### Forbidden/Banned Features:

* C++98: exceptions
* C++98: rtti
* C++11: global new/delete malloc/free
* C++11: the STL (except for type_traits/stdint/and other 'compiler' header files
* C++11: the CRT
* C++11: move semantics
* C++11: rvalue references
* C++11: forwarding references
* C++11: initializer lists
* C++11: auto
* C++11: lambda expressions
* C++11: std::thread
* C++11: smart pointers (unique_ptr/shared_ptr/weak_ptr)
* C++11: std::chrono
* C++11: std::async
* C++14: generic lambda expressions
* C++14: lamda capture initializers* C++17:
* C++14: return type deduction
* C++14: decltype(auto)
* C++14: variable templates
* C++14: builtin user-defined literals
* C++14: compile-time integer sequences
* C++17: Declaring non-type template parameters with auto
* C++17: folding template expressions
* C++17: constexpr lambda
* C++17: structured bindings

### Unclear/unchecked Features:
* C++17: template argument deduction for class templates (check support first)

## Guidelines

### Dynamic memory allocation

* All memory allocations have to use the MemoryAllocator interface (Either directly or with functions like __newObject__)

### Inheritance

* Please try to minimize the use of inheritance. It usually is not worth the trouble. It's ok to use inheritance for Interfaces (like MemoryAllocator) otherwise you should use composition instead.

### Callbacks/virtual functions

* Try to minimize use of callbacks and/or virtual functions because it makes the call-site a lot more complex -> As a callback can do practically anything it's really hard to parallelize callbacks and/or make assumptions about data flow

## Formatting

### File Names

* __.hpp__, __.cpp__, __.inl__ for C++ source files
* __.cxx__, __.hxx__ for C++ Source files using MS UWP Extensions
* __.ds__, __.crc__, __.shader__, __.hlsl__, __.lace__ for various other sources
* lower case with "__\___" between words (eg: __base\_memory\_allocator.hpp__)

### Namespaces

* lowercase, with "__\___"
* namespace __keen__ scopes everything
* don't use __using__-directives outside of classes, functions or other namespaces

### Variable Names

* only __a__, __b__, __c__, __i__, __j__, __k__, __u__, __v__, __w__, __x__, __y__, __z__ are allowed as one-char variables
* lowercase (eg: __lightDirection__)
* pointer variables start with __p__, pointerpointer with __pp__ (eg: __pValue__, __ppData__)
* class member variables start with __m___ (eg: __m\_pData__)
* struct member variables have no prefix (eg: __size__)
* static variables start with __s___ (eg: __s\_pTempFolder__)
* global variables (try to avoid!) start with __g___ (eg: __g\_badIdea__)
* don't use one-char variables in bigger for-loops
* self-explaining names (__position__ instead of __pos__, __direction__ instead of __dir__)
* pointer arrays have no __p__ prefix (eg: "__char* names[] = {...}__")

### Constants names

* Don't use enums for magic numbers
* Use constexpr: __constexpr size\_t MaxPlayers = 4u;__
* Uppercase first character

### Function Names

* case: __createYRotation__, __createGlesBuffer__, __createAabb__
* always start with a verb
* __get...__ only for simple getter with few logic (otherwise use __find/calculate/...__)

### Function Parameter

* output parameter always as pointer (no nonconst references allowed)
* output parameters first
* call by value for all parameter with __sizeof() <= 16u__
* only one bool parameter allowed, use flags or parameter struct instead
* flags parameter is always __uint32__
* assert all pointer parameter if required

### Function Return Values

* use __bool__ only for simple results, __ErrorId__ otherwise
* use __Result<T\>__ for small types
* no nonconst references allowed

### Classes

* try to avoid, use private structs instead
* use explicit constructors
* use virtual destructors when using it polymorph
* align function names and member variable names

### .hpp/.inl

* use file name defines instead of __pragma once__ in __.hpp/.inl__:
  `#ifndef KEEN_FILE_NAME_EXT #define KEEN_FILE_NAME_EXT ... #endif`
* headers should be self contained (the header has to include all required headers itself)
* use forward declaration instead of includes in header files if possible
* implement inline and template functions in __.inl__ file
* create __.inl__ file beside __.cpp__, include with relative path from __.hpp__
* Don't implement one-line functions in declaration

### .cpp

* include own header file first
* sort include files by name
* declare local functions __static__ and forward
* don't implement functions in "__namespace xyz { }__" scope
* always put __static__ variables in namespace

### Code

* use tabs, tab-size == 4__
* no "__a = b = c__" assignments
* no "__int a, b, c__" declaration
* no "if__( var == true)__" or "__if( false == var )__"
* use __KEEN\_ON/OFF__ and __KEEN\_USING__ for feature define declarations
* __size\_t__ has different sizes, be aware of that!
* use "__( )__" braces in expressions, they don't hurt
* use __nullptr__ instead of __0__ for pointer
* don't declare big variables on the stack
* compiletime-assert all enum related arrays
* don't forget _default: case_ in switches
* use empty lines to let your code breath
* use __u__ postfix for unsigned literals (eg: __26u__)
* declare everything possible __const__ or __constexpr__
* use the name 'Parameters' instead of 'Parameter' for parameter structs

### Systems

* use __createXYZ__, __destroyXYZ__, __updateXYZ__ function names
* use a parameter struct for creation if more than 2 parameters
