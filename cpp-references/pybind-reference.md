How to write a python binding, for your C++ code, using pybind11 library?
=========================================================================

Programmers have increasingly inclined towards using python for various
complex applications over the years. The easy to understand code, and
the wide adaptation of various libraries makes it convenient for both
new as well as established developers to easily build the program of
their will. However, the convenience of coding with high-level language
such as python comes at the cost of performance degradation, this is
because the more human readable that a programming language is the more
computational cost is required to convert it to machine readable format.
This is why heavy libraries such as NumPy, although written for python,
are actually written in C++ with access in python with the help of
bindings, hence providing structured and quick computation at low-level
yet highly convenient to the user.

This post is intended to be a beginner’s guide to writing their own
python binding using pybind11 library, which maps various core C++
functionalities to python, some of which will be discussed and used in
this post. We will build our own python binding for a matrix class that
provides various methods for construction and operations.

Step 1: Build the pybind11 library
----------------------------------

The building process discussed here was tested in Ubuntu 18.04.4 LTS
Operating System.

### Dependencies

The following dependencies must be met before building the pybind11
library.

-   Clang / LLVM Compiler with C++11 support, GCC 4.8 or newer

-   Python3, python3-dev (tested with v3.6.9)

-   Cmake (tested with v3.13)

### Build from Source

Pybind11 source can be cloned from the repository.
```
$ git clone https://github.com/pybind/pybind11
```
Then the following code can be executed to build and install the
pybind11 library.
```
$ cd pybind11
$ mkdir build
$ cd build
$ cmake ..
$ make -j4
$ sudo make install
```
Note: The make install command copies the pybind11 library to a global
system location which has to be added as a environment variable. This
can be avoided by adding pybind11 library as sub-module in required
projects.

Step 2: Write the code for Matrix class and its python binding
--------------------------------------------------------------

We are using a simple class named Matrix with multiple constructors and
multiple overloaded operators to demonstrate python binding. The actual
binding code is included in a separate header file named “mainpy.cpp”,
this file is used to generate the module that can be imported into
python to use the bindings created using pybind11.

### Matrix implementation subject to binding

The C++ Matrix class has basic constructors and overloaded operators
declared in “matrix.hpp” as follows
```
#include <iostream>
#include <vector>
class Matrix{
	int nr;
	int nc;
	public:
	enum INIT_PARAM{ONES = 1,ZEROS = 0};
	std::vector<std::vector<int>> mat;
	Matrix();
	Matrix(int r, int c, INIT_PARAM ini = ZEROS);
	void setValues(std::vector<int> l);
	Matrix operator +(const Matrix&) const;
	Matrix operator -( const Matrix&)const;
	Matrix operator *( const Matrix &mat1)const;
	int rows() const;
	int cols() const;
	void print();
};
```
We are also using a custom exception class to handle errors in matrix
operations. It is declared in “matexp.hpp” as follows
```
#include <exception>

class MatExp: public std::exception{
	public:
	virtual const char* what() const throw();
};
```
### Python Binding Implementation

We implement python binding by using pybind11 header-only library to map
each constructor, module and operation to python. This has been
performed in the code as follows, included in “mainpy.cpp” file.
```
#include "matrix.hpp"
#include "matexp.hpp"
#include <pybind11/pybind11.h>
#include <pybind11/operators.h>
namespace py = pybind11;
PYBIND11_MODULE(matrix, m){
	py::class_<Matrix>(m, "Matrix")
		.def(py::init([] () {return new Matrix(); }))
		.def(py::init([] (int r, int c) {return new Matrix(r,c); }))
		.def(py::init([] (int r, int c, Matrix::INIT_PARAM ini){ return new Matrix(r,c,ini); }))
		.def(py::init([] (int r, int c, py::list l){
			Matrix t(r,c);
			std::vector<int> temp;
			for (py::handle i : l){
				temp.push_back(i.cast<int>());
			}
			t.setValues(temp);
			return t;}))
		.def(py::self + py::self)
		.def(py::self - py::self)
		.def(py::self * py::self)
		.def("rows", &Matrix::rows)
		.def("cols",&Matrix::cols)
		.def("print",&Matrix::print);
	
	py::enum_<Matrix::INIT_PARAM> (m, "INIT_PARAM")
		.value("ZEROS",Matrix::INIT_PARAM::ZEROS)
		.value("ONES",Matrix::INIT_PARAM::ONES)
		.export_values();

	py::exception<MatExp> exc(m, "MatrixError");
}
```
This code includes the Matrix and exception (MatExp) classes declared in
“matrix.hpp” and “matexp.hpp” files respectively. The pre-defined
pybind11 types such as ```class_```, ```init``` (constructor) and ```enum_``` are
defined in “pybind11” namespace (accessed as “py”) in “pybind/pybind.h”
header file. Similarly, operators such as “self”, used for accessing an
object implicitly for purpose of operator overloading, are declared in
the same “pybind11” namespace in “pybind/operators.h” header file.

**Module**

The module can be named by using ```PYBIND11_MODULE(matrix,m){}``` wrapper,
where the first argument “matrix” is the name to be used while importing
the module in python, and the second argument “m” is the name used to
access the module within this wrapper. All the classes, variables,
methods, enumerations, exceptions and other types to use binding must be
defined within this wrapper.

**Class**

The method ```class<typename>(access_name, method_class_name)```
can be used to create binding for a class. Every member of the class
that requires binding has to be mentioned by using ```.def()``` attribute of
this method.

**Constructors**

The constructors can be mapped into ```init()``` method of pybind11 within
```.def()``` attribute of the class to be constructed, as shown in the code.
```init()``` method may call any constructor defined by the given class.

**Overloading**

Operator overloading can be bound by defining the operation on implicit
operator ```self```, used to access the current class object, in pybind11 namespace.

**Other Methods**

All other methods can be mapped by using ```.def()``` attribute of the class
by passing two arguments: name of the method to be used for calling in
python and the reference to the function to be called (using &
operator).

**Enumerations**

The enumerations of a class can be provided python binding in a module
by calling ```enum_<typename>(reference_names)``` method and its
attribute value() to define each value to be wrapped using the
enumeration. The values must be exported using ```export_values()```
attribute of the ```enum_``` method.

**Exceptions**

The standard (built-in) and custom exceptions in C++ implementation can
be mapped into python by using ```pybind11::exception<typename>
exc(reference_names)``` method. A custom translator can also be registered
for by calling the following wrapper.
```
py::register_exception_translator([] (std::exception\_ptr p){}
```
**Python Objects (PyObject) and Handle**

Sometimes python objects such as list and dict defined in python might
be needed to be accessed in C++, these can be translated into PyObject
and its inherited classes defined by cpython. pybind11 also supports the
following python objects (inheriting from ```PyObject``` class) that can be
handled from C++
```
bool_, buffer, bytes, capsule, dict, dtype, exception<type>,
float_, function, generic_type, int_, iterable, iterator, list,
memoryview, module, none, sequence, set, slice, staticmethod, str,
tuple, weakref
```
These objects may be used to take inputs from a python object or modify
the python object from within C++.

The python object (such as pybind11::list as shown in example) can be
iterated by using Handle class that can be cast into desired C++ type
using ```cast<typename>()``` method.

Step 3: Building the Binding
----------------------------

The python module created by using pybind11 library can be built
manually or by using Cmake.

### Using CMake

The following “CMakeLists.txt” is used to build the pybind11 module that
can be imported into python.
```
cmake_minimum_required(VERSION 3.1.0)
project (matrix_operations)
find_package(pybind11 REQUIRED)
pybind11_add_module(matrix mainpy.cpp matrix.cpp)
add_executable (matrixop main.cpp matrix.cpp)
pybind11_add_module(module_name SOURCES) is used to build the python
module using the sources listed.
```
Note: ```find_package(pybind11 REQUIRED)``` may be used only when the
pybind11 library has been built globally. If the library is included as
a sub-module, ```add_subdirectory(pybind11)``` should be used instead.

### Manual Build

The module may also be built manually as follows.
```
$ c++ -O3 -Wall -shared -std=c++11 -undefined dynamic_lookup `python3 -m pybind11 --includes` mainpy.cpp -o  matrix`python3-config --extension-suffix`
```
Step 4: Output
--------------

The built module can be imported by invoking python3 in the same
directory as the built shared-object (.so) file. The output is as
follows
```
aashutosh@aashutosh-msi:~/ekbana/internship/pybind-matrix/build$ python3
Python 3.6.9 (default, Nov 7 2019, 10:44:02)
[GCC 8.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>import matrix
>>> m = matrix.Matrix(3,2,\[1,2,3,4,5,6\])
>>> n = matrix.Matrix(2,3,\[1,-1,-1,1,1,-1\])
>>> o = m\*n
>>> o.print()
3 1 -3
7 1 -7
11 1 -11
>>>
```
Looks Good!

Further documentation and references can be found here
[*https://pybind11.readthedocs.io/en/stable/intro.html*](https://pybind11.readthedocs.io/en/stable/intro.html)
