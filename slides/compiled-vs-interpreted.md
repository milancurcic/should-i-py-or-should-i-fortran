<section>

### Compiled vs Interpreted
</section>

<section>

### Compiled vs Interpreted

* Fortran is a _compiled_ language
* Python is an _interpreted_ language

**=> This impacts how you _write_, _build_, and _distribute_ your software**
<!-- .element: class="fragment" -->

</section>


<section>

### Building a Fortran executable

Building an executable program involves a few steps

Source code -> compiler -> linker -> executable <!-- .element: class="fragment" -->
</section>

<section>

### Building a Fortran library

Alternatively, building a library is a tad simpler

Source code -> compiler -> library <!-- .element: class="fragment" -->

**=>  Longer feedback cycles, slower development** <!-- .element: class="fragment" -->

</section>


<section>

### Typical development flow

1. Write some code and save to a file
2. Does it compile? Yes, go to 3; no, go to 1
3. Does it run as expected? If no, go to 1
</section>

<section>

### Compiler aids development

Fortran code won't build until syntactically and semantically correct <!-- .element: class="fragment" -->

You'll spend a lot of time making the compiler happy <!-- .element: class="fragment" -->

Once it compiles, your program is more likely to be correct <!-- .element: class="fragment" -->

Now, all you gotta worry about is that your program runs correctly <!-- .element: class="fragment" -->

</section>

<section>

### Fortran is compiled: Pros and cons

* Creates faster machine code 😀
* Slower development 😴
* Binaries specific to target machine ⚙
* Modules often incompatible between:
  - compiler vendors
  - compiler versions of same vendor
</section>

<section>

### Python is an interpreted language

```
$ python3
Python 3.6.8 (default, Mar 21 2019, 10:08:12)
[GCC 8.3.1 20190223 (Red Hat 8.3.1-2)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> print('hello python!')
hello python!
>>> 12 + 23
35
>>>
```

</section>

<section>

### Short feedback cycles -> rapid prototyping

```python
>>> def circle_area(r):
...     PI = 3.14159256
...     return r**2 * PI
...
>>> circle_area(0.4)
0.5026548096000001
```
</section>

<section>

### Interpreter aids exploration and discovery

* Greater development productivity, especially early in the project
* Short feedback cycles -> rapid prototyping
* From idea to minimal working code in short time
* Great for testing ideas and quick experiments

</section>
