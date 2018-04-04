# keen core C++ coding standards v0.2

### Prime Directive
* __code has to be simple, fast and self-explanatory__
* No surprises is the main principle  

### File Names

* __.hpp__, __.cpp__, __.inl__ for C++ source files
* __.cxx__, __.hxx__ for C++ Source files using MS UWP Extensions
* __.ds__, __.crc__, __.shader__, __.hlsl__, __.lace__ for various other sources
* lower case with "__\___" between words (eg: __base\_memory\_allocator.hpp__)

### Namespaces

* lowercase, without "__\___"
* namespace __keen__ scopes everything
* don't use __using__-directives outside of classes, functions or other namespaces 

### Exceptions

* The use of C++ Exceptions is forbidden

### RTTI

* The use of C++ RTTI is forbidden

### Inheritance

* Please try to minimize the use of inheritance. It usually is not worth the trouble. It's ok to use inheritance for Interfaces (like MemoryAllocator) otherwise you should use composition instead.

### Dynamic memory allocation

* All memory allocations have to use the MemoryAllocator interface (Either directly or with the __KEEN\_NEW__ macro)

### C++11 features

* We currently don't allow any C++11 only features to be used (may change in the future)

### Variable Names

* only __a__, __b__, __c__, __i__, __j__, __k__, __u__, __v__, __w__, __x__, __y__, __z__ are allowed as one-char variables
* lowercase (eg: __lightDirection__)
* pointer variables start with __p__, pointerpointer with __pp__ (eg: __pValue__, __ppData__)
* class member variables start with __m___ (eg: __m\_pData__)
* struct member variables have no prefix (eg: __size__)
* static variables start with __s___ (eg: __s\_pTempFolder__)
* global variables (try to avoid!) start with __g___ (eg: __g\_badIdea__)
* dont't use one-char variables in bigger for-loops
* self-explaining names (__position__ instead of __pos__, __direction__ instead of __dir__)
* pointer arrays have no __p__ prefix (eg: "__char* names[] = {...}__")

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
* declare everything possible __const__
* use the name 'Parameters' instead of 'Parameter' for parameter structs

### Systems

* use __createXYZ__, __destroyXYZ__, __updateXYZ__ function names
* use a parameter struct for creation if more than 2 parameters
