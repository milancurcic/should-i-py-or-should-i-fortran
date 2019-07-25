<section>

### So, what is Fortran so great for?
</section>

<section>

### Arrays

The **only** built-in data structure
<!-- .element: class="fragment" -->

Language designed around easy-to-use, whole-array operations
<!-- .element: class="fragment" -->

```fortran
real :: a(100,100), b(100,100), c(100, 100)
...
c = 2 * a * b ! operators work element-wise
```
<!-- .element: class="fragment" -->
</section>

<section>

### Pure functions

Fortran lets you define pure functions to prevent side-effects
<!-- .element: class="fragment" -->

```fortran
pure real function vapor_saturation_pressure(temperature) result(res)
  ! Returns water vapor saturation pressure (Pa)
  ! given input temperature (C)
  real, intent(in) :: temperature
  res = 6.1094e2 * exp(17.625 * temperature / (temperature + 243.04))
end function
```
<!-- .element: class="fragment" -->
</section>


<section>

### Pure functions

Help you write cleaner, more composable code

Writing pure functions helps compiler optimize code for performance
<!-- .element: class="fragment" -->

Pure functions lend themselves to parallelism
<!-- .element: class="fragment" -->

**=> Use them whenever you can!**
<!-- .element: class="fragment" -->

</section>

<section>

### Elemental functions

You can define functions to operate on both scalars and arrays of any shape and size

```fortran
pure elemental real function vapor_saturation_pressure(temperature) result(res)
  ! Returns water vapor saturation pressure (Pa)
  ! given input temperature (C)
  real, intent(in) :: temperature
  res = 6.1094e2 * exp(17.625 * temperature / (temperature + 243.04))
end function
```
<!-- .element: class="fragment" -->
</section>

<section>

### Elemental functions

You can now do

```fortran
print *, vapor_saturation_pressure(0.)
! outputs:   610.940002
```

as well as

```fortran
print *, vapor_saturation_pressure([0., 10., 20., 30.])
! outputs:   610.940002       1226.02063       2333.44116       4236.65088
```
</section>


<section>

### Easy distributed parallelism with coarrays

* Introduced with Fortran 2008
* Native, parallel data structure
* Syntax independent from underlying architecture or network
* Supported in GNU, Intel, and Cray compilers
</section>


<section>

### Easy distributed parallelism with coarrays

```fortran
program coarrays_example
  implicit none
  integer, allocatable :: q(:)[:]

  ! allocate a 10-element integer array
  ! on all parallel images
  allocate(q(10)[*])
  q = this_image()

  ! Remote copy from image 1 to image 2
  if (this_image() == 1) q(:)[2] = q(:)

end program coarrays_example
```
</section>


<section>

### Math looks like math

Take the shallow water system of equations for example:

$$
\dfrac{\partial u}{\partial t} = - u \dfrac{\partial u}{\partial x} - g \dfrac{\partial h}{\partial x}
$$
$$
\dfrac{\partial h}{\partial t} = - \dfrac{\partial \left[ u(H + h)\right]}{\partial x}
$$

</section>


<section>

$$
\dfrac{\partial u}{\partial t} = - u \dfrac{\partial u}{\partial x} - g \dfrac{\partial h}{\partial x}
$$
$$
\dfrac{\partial h}{\partial t} = - \dfrac{\partial \left[ u(H + h)\right]}{\partial x}
$$

```fortran
time_loop: do n = 1, num_time_steps

  ! compute u at next time step
  u = u - (u * diff(u) + g * diff(h)) / dx * dt

  ! compute h at next time step
  h = h - diff(u * (hmean + h)) / dx * dt

end do time_loop
```
</section>

<section>

```fortran
time_loop: do n = 1, num_time_steps

  ! compute u at next time step
  u = u - (u * diff(u) + g * diff(h)) / dx * dt

  ! compute h at next time step
  h = h - diff(u * (hmean + h)) / dx * dt

end do time_loop
```

* `u` and `h` arrays distributed across parallel processors
* Parallel arithmetic operators
* Synchronization on assignment

</section>

<section>

### Not everything's great though...

* Fortran carries a lot of baggage from its rich and long history
* Tedious and archaic I/O, weak string support
* Small standard library with focus on numerics
* No built-in collections beyond arrays
* No reference implementation

**=> A niche language for niche applications**
<!-- .element: class="fragment" -->

</section>

<section>

### Fortran is a great solution for a small set of problems

* Arrays and whole-array arithmetic are _like nothing else_ IMO
* Native, distributed parallelism with coarrays
* Math looks like math
* Small language that is easy to learn
</section>
