# Homework Problems

{% tabs %}
{% tab title="2.55 ◆" %}
Compile and run the sample code that uses `show_bytes` (file `show-bytes.c`) on different machines to which you have access. Determine the byte orderings used by these machines.
{% endtab %}

{% tab title="2.56 ◆" %}
Try running the code for `show_bytes` for different sample values.
{% endtab %}

{% tab title="2.57 ◆" %}
Write procedures `show_short`, `show_long`, and `show_double` that print the byte representations of C objects of types `short`, `long`, and `double`, respectively. Try these out on several machines.
{% endtab %}

{% tab title="2.58 ◆◆" %}
Write a procedure `is_little_endian` that will return 1 when compiled and run on a little-endian machine, and will return 0 when compiled and run on a big-endian machine. This program should run on any machine, regardless of its word size.
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Solution 2.55" %}
Litte endian.
{% endtab %}

{% tab title="Solution 2.56" %}
```c
int x = 0x12345678;
int* p = &x;
double y = 1.0;
show_bytes((byte_pointer) &x, sizeof(int));
show_bytes((byte_pointer) p, sizeof(int*));
show_bytes((byte_pointer) &y, sizeof(double));
```

Running result:

```
[jack@manager-master CPlusPlus_demo]$ ./test
 78 56 34 12
 78 56 34 12 24 00 5f df
 00 00 00 00 00 00 f0 3f
```
{% endtab %}

{% tab title="Solution 2.57" %}
```c
using byte_pointer = unsigned char*;

void show_bytes(byte_pointer start, size_t len) {
    for (int i = 0; i < len; i++)
        printf(" %.2x", start[i]);
    printf("\n");
}

void show_short(short x) {
    show_bytes((byte_pointer) &x, sizeof(short));
}

void show_long(long x) {
    show_bytes((byte_pointer) &x, sizeof(long));
}

void show_double(double x) {
    show_bytes((byte_pointer) &x, sizeof(double));
}
```
{% endtab %}

{% tab title="Solution 2.58" %}
```c
bool is_little_endian() {
    short x = 0x1234;
    unsigned char* y = (unsigned char*) &x;
    return y[0] == 0x34;
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="2.59 ◆◆" %}
Write a C expression that will yield a word consisting of the least significant byte of x and the remaining bytes of y. For operands x = 0x89ABCDEF and y = 0x76543210, this would give 0x765432EF.
{% endtab %}

{% tab title="2.60 ◆◆" %}
Suppose we number the bytes in a w-bit word from 0 (least significant) to w/8 − 1 (most significant). Write code for the following C function, which will return an unsigned value in which byte i of argument x has been replaced by byte b:

`unsigned replace_byte (unsigned x, int i, unsigned char b);`

Here are some examples showing how the function should work:

replace\_byte(0x12345678, 2, 0xAB) --> 0x12AB5678

replace\_byte(0x12345678, 0, 0xAB) --> 0x123456AB
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Solution 2.59" %}
`(x & 0xff) | (y & ~0xff)`
{% endtab %}

{% tab title="Solution 2.60" %}
```c
unsigned replace_byte(unsigned x, int i, unsigned char b) {
    if (i < 0) {
        return x;
    }
    int replace_index = i % sizeof(unsigned);
    
    /* Byte i of x set to 0 */
    x &= ~(0xff << (replace_index <<3));
    /* Construct y which byte i equal to b and other bytes set to 0 */
    unsigned y = b << (replace_index <<3);
    /* Combine x and y */
    return x | y;
}
```
{% endtab %}
{% endtabs %}

#### Bit-Level Integer Coding Rules

In several of the following problems, we will artificially restrict what programming constructs you can use to help you gain a better understanding of the bit-level, logic, and arithmetic operations of C. In answering these problems, your code must follow these rules:

* **Assumptions**
  * Integers are represented in two’s-complement form.
  * Right shifts of signed data are performed arithmetically.
  * Data type `int` is w bits long. For some of the problems, you will be given a specific value for w, but otherwise, your code should work as long as w is a multiple of 8. You can use the expression `sizeof(int)<<3` to compute w.
* **Forbidden**
  * Conditionals (`if` or `?:`), loops, switch statements, function calls, and macro invocations.&#x20;
  * Division, modulus, and multiplication.
  * Relative comparison operators (<, >, <=, and >=).
* **Allowed operations**
  * All bit-level and logic operations.
  * Left and right shifts, but only with shift amounts between 0 and w − 1.
  * Addition and subtraction.
  * Equality (==) and inequality (!=) tests. (Some of the problems do not allow these.)
  * Integer constants `INT_MIN` and `INT_MAX`.
  * Casting between data types `int` and `unsigned`, either explicitly or implicitly.

Even with these rules, you should try to make your code readable by choosing descriptive variable names and using comments to describe the logic behind your solutions. As an example, the following code extracts the most significant byte from integer argument x:

```c
/* Get most significant byte from x */
int get_msb(int x) {
    /* Shift by w-8 */
    int shift_val = (sizeof(int)-1)<<3;
    /* Arithmetic shift */
    int xright = x >> shift_val;
    /* Zero all but LSB */
    return xright & 0xFF;
}
```

{% tabs %}
{% tab title="2.61" %}
Write C expressions that evaluate to 1 when the following conditions are true and to 0 when they are false. Assume x is of type `int`.

A. Any bit of x equals 1.

B. Any bit of x equals 0.

C. Any bit in the least significant byte of x equals 1.

D. Any bit in the most significant byte of x equals 0.

Your code should follow the bit-level integer coding rules (page 164), with the additional restriction that you may not use equality (==) or inequality (!=) tests.
{% endtab %}

{% tab title="2.62" %}
Write a function `int_shifts_are_arithmetic()` that yields 1 when run on a machine that uses arithmetic right shifts for data type `int` and yields 0 otherwise. Your code should work on a machine with any word size. Test your code on several machines.
{% endtab %}

{% tab title="2.63" %}
Fill in code for the following C functions. Function `srl` performs a logical right shift using an arithmetic right shift (given by value `xsra`), followed by other operations not including right shifts or division. Function `sra` performs an arithmetic right shift using a logical right shift (given by value `xsrl`), followed by other operations not including right shifts or division. You may use the computation `8*sizeof(int)` to determine w, the number of bits in data type `int`. The shift amount k can range from 0 to w − 1.

```c
unsigned srl(unsigned x, int k) {
    /* Perform shift arithmetically */
    unsigned xsra = (int) x >> k;
.
.
.
.
.
.
}

int sra(int x, int k) {
    /* Perform shift logically */
    int xsrl = (unsigned) x >> k;
.
.
.
.
.
.
}
```
{% endtab %}

{% tab title="2.64" %}
Write code to implement the following function:

```c
/* Return 1 when any odd bit of x equals 1; 0 otherwise. Assume w=32 */
int any_odd_one(unsigned x);
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Solution 2.61" %}
```c
A. !(~0 - x)
B. !(x - 0)
C. !(0xff - x&0xff)
D. !((x & (0xff << ((sizeof(int)-1) << 3))) - 0)
```
{% endtab %}

{% tab title="Solution 2.62" %}
```c
bool int_shifts_are_arithmetic() {
    
}
```
{% endtab %}

{% tab title="Solution 2.63" %}

{% endtab %}

{% tab title="Solution 2.64" %}

{% endtab %}
{% endtabs %}
