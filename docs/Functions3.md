# Functions

## Declaration
In Tego all functions return something. This function has no arguments:

    def f() -> R.

This function has one argument:

    def g(A) -> R.

But a function may take more than one:

    def h(A, B, C) -> R.

If a function is a relation, you may want to use the alternative syntax (for functions with one or more arguments):

    def A -g-> R.

    def B, C |- A -h-> R.

## Definition
Once you've declared a function, you have to define it.


    f() -> r.

    g(a) -> r.

    h(a, b, c) -> r.

If a function is a relation, you may want to use the alternative syntax (for functions with one or more arguments):

    a -g-> r.

    b, c |- a -h-> r.

## Application
You can apply a function.

    f()

    g(10)

    h("a", 10, true)

And store it in a variable.

    v0 := f()
    v1 := g(10)
    v2 := h("a", 10, true)

If you want to apply a reduction, you may want to use the alternative syntax which also stores the result:

    10 -g-> v1

    10, true |- "a" -h-> v2

    n |- m -mul-> k;
    m |- k -add-> l.

## Composition
You can compose functions.

    g(g(10))

    add(mul(m, n), m)

    m |- n |- m -mul-> -add-> l.
