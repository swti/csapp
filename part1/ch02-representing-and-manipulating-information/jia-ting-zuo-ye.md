# Homework Problems

## Problems

{% tabs %}
{% tab title="2.55 ◆" %}

{% endtab %}

{% tab title="2.56 ◆" %}

{% endtab %}

{% tab title="2.57 ◆" %}

{% endtab %}

{% tab title="2.58 ◆◆" %}

{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="2.59 ◆◆" %}

{% endtab %}

{% tab title="2.60 ◆◆" %}

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
