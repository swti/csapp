# Homework Problems

{% tabs %}
{% tab title="2.55 ◆" %}
Compile and run the sample code that uses `show_bytes` (file `show-bytes.c`) on different machines to which you have access. Determine the byte orderings used by these machines.
{% endtab %}

{% tab title="2.56 ◆" %}
Try running the code for show\_bytes for different sample values.
{% endtab %}

{% tab title="2.57 ◆" %}
Write procedures show\_short, show\_long, and show\_double that print the byte representations of C objects of types short, long, and double, respectively. Try these out on several machines.
{% endtab %}

{% tab title="2.58 ◆◆" %}
Write a procedure is\_little\_endian that will return 1 when compiled and run on a little-endian machine, and will return 0 when compiled and run on a big- endian machine. This program should run on any machine, regardless of its word size.
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Solution 2.55" %}
litte endian
{% endtab %}

{% tab title="Solution 2.56" %}

{% endtab %}

{% tab title="Solution 2.57" %}

{% endtab %}

{% tab title="Solution 2.58" %}

{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="2.59 ◆◆" %}
Write a C expression that will yield a word consisting of the least significant byte of x and the remaining bytes of y. For operands x = 0x89ABCDEF and y = 0x76543210, this would give 0x765432EF.
{% endtab %}

{% tab title="2.60 ◆◆" %}
Suppose we number the bytes in a w-bit word from 0 (least significant) to w/8 − 1 (most significant). Write code for the following C function, which will return an unsigned value in which byte i of argument x has been replaced by byte b:

unsigned replace\_byte (unsigned x, int i, unsigned char b);

Here are some examples showing how the function should work:

replace\_byte(0x12345678, 2, 0xAB) --> 0x12AB5678

replace\_byte(0x12345678, 0, 0xAB) --> 0x123456AB
{% endtab %}
{% endtabs %}

#### Bit-Level Integer Coding Rules

In several of the following problems, we will artificially restrict what programming constructs you can use to help you gain a better understanding of the bit-level, logic, and arithmetic operations of C. In answering these problems, your code must follow these rules:

* Assumptions
  * Integers are represented in two’s-complement form.
  * Right shifts of signed data are performed arithmetically.
  * Data type `int` is w bits long. For some of the problems, you will be given a specific value for w, but otherwise, your code should work as long as w is a multiple of 8. You can use the expression `sizeof(int)<<3` to compute w.
* Forbidden
  * Conditionals (`if` or `?:`), loops, switch statements, function calls, and macro invocations.&#x20;
  * Division, modulus, and multiplication.
  * Relative comparison operators (<, >, <=, and >=).
* Allowed operations
  * All bit-level and logic operations.
  * Left and right shifts, but only with shift amounts between 0 and w − 1.
  * Addition and subtraction.
  * Equality (==) and inequality (!=) tests. (Some of the problems do not allow these.)
  * Integer constants `INT_MIN` and `INT_MAX`.
  * Casting between data types int and unsigned, either explicitly or implicitly.

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
