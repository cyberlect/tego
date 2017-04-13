# Functions

## Function Declaration
A function declaration specifies the type of input term, arguments, and output term.

    def eval(Context): Exp -> Val.


## Function Definition
A function definition provides one of possibly many implementations of the function.

    eval(C): Int(i) -> IntV(i).

    eval(C): Var(n) -> v
    where C.lookup(n) -> v.


## Function Application
To apply a function `eval`:

    eval(e, C) -> v

Or the default function:

    (e, C) -> v

Or with the turnstile notation:

    C |- e -eval-> v

Or the default function:

    (e, C) --> v

In general, for a function `f(a0, a1, a2, a3, a4)` the notation is:

    a1, a2 |- f(a0, a3, a4) --> r

Or:

    a1, a2 |- a0 -f(a3, a4)-> r
