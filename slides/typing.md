<section>

### Typing

Fortran: Static, strong, manifest <!-- .element: class="fragment" -->

Python: Dynamic, strong, inferred <!-- .element: class="fragment" -->

</section>

<section>

### Static typing

Types are checked at compile-time

```fortran
program calculate_vapor_pressure

  implicit none
  integer :: i

  do i = 0, 30, 5
    print *, real(i), vapor_saturation_pressure(real(i))
  end do

contains

  real function vapor_saturation_pressure(temperature) result(res)
    ! Returns water vapor saturation pressure (Pa)
    ! given input temperature (C)
    real, intent(in) :: temperature
    res = 6.1094e2 * exp(17.625 * temperature / (temperature + 243.04))
  end function

end program calculate_vapor_pressure
```
</section>

<section>

Let's compile and run it:

```
gfortran calculate_vapor_pressure.f90
./a.out
   0.00000000       610.940002    
   5.00000000       871.559570    
   10.0000000       1226.02063    
   15.0000000       1701.98303    
   20.0000000       2333.44116    
   25.0000000       3161.73633    
   30.0000000       4236.65088   
```
</section>

<section>

Strong typing: What happens if we call this function with a different type?

```fortran
do i = 0, 30, 5
  print *, real(i), vapor_saturation_pressure(i)
end do
```
<!-- .element: class="fragment" -->

Let's try to compile it again:
<!-- .element: class="fragment" -->

```
gfortran calculate_vapor_pressure.f90
calculate_vapor_pressure.f90:7:21:

     print *, real(i), vapor_saturation_pressure(i)
                     1
Error: Type mismatch in argument ‘temperature’ at (1); passed INTEGER(4) to REAL(4)
```
<!-- .element: class="fragment" -->
</section>


<section>

### Static & strong typing

Types are checked at compile-time, before execution
<!-- .element: class="fragment" -->

Are argument types compatible at functions calls?
<!-- .element: class="fragment" -->

Help catch bugs early & write correct programs
<!-- .element: class="fragment" -->
</section>

<section>

### Let's try the same in Python

Types are checked at run-time

```python
from math import exp

def vapor_saturation_pressure(temperature):
    """ Returns water vapor saturation pressure (Pa)
    given input temperature (C)"""
    return 6.1094e2 * exp(17.625 * temperature / (temperature + 243.04))

for i in range(0, 35, 5):
    print(float(i), vapor_saturation_pressure(float(i)))
```
</section>

<section>

```
python3 calculate_vapor_pressure.py
0.0 610.94
5.0 871.5595494938506
10.0 1226.020635023486
15.0 1701.9828147155868
20.0 2333.4406230993577
25.0 3161.7360356966915
30.0 4236.650251295475
```
</section>

<section>

Can we break it?

```python
for i in range(0, 35, 5):
    print(float(i), vapor_saturation_pressure(float(i)))

print('Will it take a string?', vapor_saturation_pressure('27'))
```
</section>

<section>

The error won't be triggered until the offending line is executed:

```
python3 calculate_vapor_pressure.py
0.0 610.94
5.0 871.5595494938506
10.0 1226.020635023486
15.0 1701.9828147155868
20.0 2333.4406230993577
25.0 3161.7360356966915
30.0 4236.650251295475
Traceback (most recent call last):
  File "calculate_vapor_pressure.py", line 11, in <module>
    print('Will it take a string?', vapor_saturation_pressure('27'))
  File "calculate_vapor_pressure.py", line 6, in vapor_saturation_pressure
    return 6.1094e2 * exp(17.625 * temperature / (temperature + 243.04))
TypeError: can't multiply sequence by non-int of type 'float'
```

</section>


<section>

### Python variables can also change types

A variable can freely change its type during the life of the program

```python
>>> a = 2 # a is now an integer
>>> type(a)
<type 'int'>
>>> a = 3.141 # a is now a float
>>> type(a)
<type 'float'>
>>> a = 'spam, eggs, and ham' # a is now a string
>>> type(a)
<type 'str'>
```

**=> More flexible, but don't shoot yourself in the foot!**
<!-- .element: class="fragment" -->
</section>


<section>

### Dynamic typing

Variable types are checked at run-time
<!-- .element: class="fragment" -->

Allows for more flexible language design
<!-- .element: class="fragment" -->

Prevents some optimizations
<!-- .element: class="fragment" -->

Bugs can go unnoticed for a long time
<!-- .element: class="fragment" -->
</section>


<section>

### Enter Python type hints and mypy

Type hints introduced in Python 3.5

Example from the [docs](https://docs.python.org/3/library/typing.html):

```python
from typing import List
Vector = List[float]

def scale(scalar: float, vector: Vector) -> Vector:
    return [scalar * num for num in vector]

# typechecks; a list of floats qualifies as a Vector.
new_vector = scale(2.0, [1.0, -4.2, 5.4])
```
</section>

<section>

### mypy: Optional static typing for Python

```
mypy --strict calculate_vapor_pressure.py
calculate_vapor_pressure.py:3: error: Function is missing a type annotation
calculate_vapor_pressure.py:9: error: Call to untyped function "vapor_saturation_pressure" in typed context
```

</section>

<section>

### mypy: Optional static typing for Python

Redefine our function with Type hints:

```
def vapor_saturation_pressure(temperature: float) -> float:
    """ Returns water vapor saturation pressure (Pa)
    given input temperature (C)"""
    return 6.1094e2 * exp(17.625 * temperature / (temperature + 243.04))
```
<!-- .element: class="fragment" -->

```
mypy --strict calculate_vapor_pressure_typed.py

```
<!-- .element: class="fragment" -->

**=> Type hints + mypy yield development benefits of static typing without sacrificing flexibility**
<!-- .element: class="fragment" -->
</section>


<section>

### How about manifest typing?

Modern Fortran requires you to declare your variables before using them
<!-- .element: class="fragment" -->

```fortran
implicit none ! enforces explicit declaration

integer :: counter
real :: a, b, c
type(Particle) :: graviolis(1e5)
```
<!-- .element: class="fragment" -->

Useful as annotation and makes code easier to understand
<!-- .element: class="fragment" -->

More verbose -> slower development
<!-- .element: class="fragment" -->
</section>


<section>

### The opposite is inferred typing

Python infers variable types on assignment
<!-- .element: class="fragment" -->

```python
pi = 3.14159256 # this is a float
greeting = 'hello' # this is a str
```
<!-- .element: class="fragment" -->

Less verbose -> faster development
<!-- .element: class="fragment" -->

Again, type annotations can help you
<!-- .element: class="fragment" -->
</section>


<section>

### Type systems takeaways

Static, manifest typing makes Fortran more rigid & verbose than Python
<!-- .element: class="fragment" -->

A Fortran program that compiles is more likely to be correct during run-time
<!-- .element: class="fragment" -->

A compromise between _flexibility_ and _type safety_
<!-- .element: class="fragment" -->

Python type hints + mypy can bring both static and manifest typing to the table -> **Best of both worlds!**
<!-- .element: class="fragment" -->

</section>
