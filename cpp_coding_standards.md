# keen games C++ Coding Standards v1.1 (09.05.2025)

## Prime Directive
* __code has to be simple, fast and self-explanatory__
* No surprises!
* Make reading/understanding of code easier than writing

## C++ Features

### Features that are mandatory / encouraged:

* C++11: static assertions -> you *can* use the `KEEN_STATIC_ASSERT` macro instead
* C++11: `nullptr` -> use it everywhere / never use `0` for pointers
* C++11: C ++ attributes: `noreturn`, `deprecated`, `nodiscard`
* C++11: `constexpr` values -> use for named constants (over enums/defines/const)
* C++11: `constexpr` functions -> use for ctors or stuff you might want to use in constexpr initialization
* C++11: explicit virtual overrides
* C++11: strongly typed enums (nearly always specify the underlying type)
* C++11: final specifier
* C++11: inline namespaces -> sure, why not.. maybe try it for versioning?
* C++11: non static data member initializers -> prefer a struct with member initializers over a class with constructor for 'public'/'parameter' structs
* C++11: range based foor loops (only for const refs/values)
* C++14: binary literals
* C++17: if with init-statement -> helpful when parsing messages `if( MyMsg* pMyMsg = castMyMessage( pMsg ); pMyMsg != nullptr )`
* C++20: `consteval`
* C++20: designated initializers (for huge parameter structs)

### 'Meh' Features which should be only used when they are really worth it in the situation

* C++11: variadic templates -> minimize use / only for formatString/scanString
* C++11: template aliasing
* C++11: decltype -> only useful in some template meta programming code (which should be minimized)
* C++11: delegating constructors -> sure, why not.. but minimizing the use of constructors is preferred
* C++11: user-defined literals -> only for library types
* C++11: default functions -> sure, why not.. but avoid 'default' functions whenever possible
* C++11: deleted functions
* C++11: range based for loops -> *can* be used but often a normal / raw loop is easier
* C++11: explicit conversion functions (if you need to write a conversion operator it probably should be explicit)
* C++11: type_traits
* C++11: move semantics (only use if you need the struct in a growing container - and you need the mtor licence!)
* C++11: lambda expressions (don't exaggerate)
* C++17: inline variables (prefer constexpr)
* C++17: nested namespaces
* C++17: constexpr if (better than alternatives, but try to minimize meta-programming)
* C++20: `[[likely]] [[unlikely]]` with `KEEN_LIKELY KEEN_UNLIKELY`
* C++79: Function Macros -> prefer template functions

### Forbidden/Banned Features:

* C++98: c arrays, use `StaticArray< T, S >` instead
* C++98: exceptions
* C++98: rtti
* C++11: global new/delete malloc/free
* C++11: the STL (except for type_traits/stdint/and other 'compiler' header files)
* C++11: the CRT
* C++11: rvalue references
* C++11: forwarding references
* C++11: initializer lists
* C++11: auto
* C++11: std::thread
* C++11: smart pointers (unique_ptr/shared_ptr/weak_ptr)
* C++11: std::chrono
* C++11: std::async
* C++14: generic lambda expressions
* C++14: lambda capture initializers
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

* all memory allocations have to use the MemoryAllocator interface (Either directly or with functions like `newObject`)
* try to use pools instead of new/delete
* think about upper memory limits

### Inheritance

* Please try to minimize the use of inheritance. It usually is not worth the trouble. It's ok to use inheritance for Interfaces (like MemoryAllocator) otherwise you should use composition instead.

### Callbacks/virtual functions

* Try to minimize use of callbacks and/or virtual functions because it makes the call-site a lot more complex -> As a callback can do practically anything it's really hard to parallelize callbacks and/or make assumptions about data flow

### Classes

* try to avoid, use private structs instead
* use explicit constructors
* use virtual destructors when using it polymorph
* align function names and member variable names

### Structs

* avoid lots of members, try to group them in other structs

### Access

* don't access an array element twice, create a local reference instead
* don't create access wall of texts, eg. access multiple struct members in deep structs (`a.b.c.x = 3u; a.b.c.y = 4u;`), create a local reference instead

### Function Parameter

* output parameter always as pointer (no nonconst references allowed)
* output parameters first
* call by value for all parameter with `sizeof() <= 16u` (use reference for const structs with cctor)
* only one bool parameter allowed, use flags or parameter struct instead
* use `Bitmask` for flags
* assert pointer parameters if you don't access them immediately 
* if you want to have your call by value parameter `const`, declare it `const` only in the function implementation

### Function Return Values

* use `bool` only for simple results, `Result< void >` otherwise
* use `Result< T >` for small types
* no nonconst references allowed

## Formatting

### File Names

* __.hpp__, __.cpp__, __.inl__ for C++ source files
* __.cxx__, __.hxx__ for C++ Source files using MS UWP Extensions
* __.ds__, __.shader__, __.hlsl__, __.lace__ for various other sources
* lower case with `_` between words (eg: `base_memory_allocator.hpp`)

### Namespaces

* lowercase, with `_`
* namespace `keen` scopes everything
* don't use `using`-directives outside of classes, functions or other namespaces

### Variable Names

* only `a`, `b`, `c`, `i`, `j`, `k`, `u`, `v`, `w`, `x`, `y`, `z` are allowed as one-char variables
* lowercase (eg: `lightDirection`)
* pointer variables start with `p`, pointerpointer with `pp` (eg: `pValue`, `ppData`)
* class member variables start with `m_` (eg: `m_pData`)
* struct member variables have no prefix (eg: `size`)
* static variables start with `s_` (eg: `s_pTempFolder`)
* global variables (try to avoid!) start with `g_` (eg: `g_badIdea`)
* don't use one-char variables in bigger for-loops
* self-explaining names (`position` instead of `pos`, `direction` instead of `dir`)
* pointer arrays have no `p` prefix

### Constants names

* Don't use enums/macros for magic numbers
* Use constexpr: `constexpr uint32 MaxPlayers = 4u;`
* Uppercase first character

### Function Names

* case: `createYRotation`, `createGlesBuffer`, `createAabb`
* always start with a verb
* `get...` only for simple getter with few logic (otherwise use `find/calculate/...`)

### .hpp/.inl

* use file name defines instead of `pragma once` in __.hpp/.inl__:

		#ifndef KEEN_VOXEL_DATA_HPP
		#define KEEN_VOXEL_DATA_HPP 
		... 
		#endif

* headers should be self contained (the header has to include all required headers itself)
* use forward declaration instead of includes in header files if possible
* implement inline and template functions in __.inl__ file
* create __.inl__ file beside __.cpp__, include with relative path from __.hpp__
* Don't implement functions in declaration to keep it small and simple!

### .cpp

* include own header file first
* sort include files by name
* declare local functions `static` and forward
* don't implement functions in `namespace xyz { }` scope
* always put `static` variables in namespace
* no-const references not allowed

### Code

* use tabs, `tab-size == 4`
* no `a = b = c` assignments
* no `int a, b, c` declaration
* Try to avoid `size_t`, `int`, `uint` types (use `uint32` for `size_t` when you know 4gib is enough)
* no `if( var == true)` or `if( false == var )`
* use `KEEN_ON/KEEN_OFF` and `KEEN_USING` for feature define declarations
* use `( )` braces in expressions, they don't hurt
* use `nullptr` instead of `0` for pointer
* don't declare big variables on the stack
* try to use `uint32` instead of `size_t`
* compiletime-assert all enum related arrays
* don't use `default:` case in switches with enum value (so the compiler can tell you missing cases)
* use `default:` case in switches otherwise
* use empty lines to let your code breath
* use `u` postfix for unsigned literals (eg: `26u`)
* declare everything possible `const` or `constexpr`
* use the name 'Parameters' instead of 'Parameter' for parameter structs
* default-initialize all values in a 'Parameters' struct
* don't use `static` local variables in functions

### Systems

* use `createXYZ`, `destroyXYZ`, `updateXYZ` function names
* use a parameter struct for creation if more than 2 parameters
* all destroy functions have to be asynchronous (call `destroyXYZ` until it returns `true`)

### Unit Tests

* create unit tests for all base functionality (it's a long way)
* try to catch the corner cases and do some pseudo random tests
