# Syntax

## Terms and Constructors
A _term_ is a data structure that is most often used in compilers and related tools for representing trees. A term has a name and arguments, which are again terms. For example, the code `4 + f(5 * x)` may be represented as a term as:

    Plus(Int("4"), Call("f", [Mul(Int("5"), Var("x"))]))

Terms in Tego are typed, which means that the type of the term and its arguments are described in a term signature. The term _signature_ for the `Plus` term above could be:

    Plus(Exp, Exp) : Exp

However, you don't write term signatures directly. Instead, the term signature is inferred from a term _constructor_ that you define. For the above signature the following constructor function could be defined:

    def Plus(lhs: Exp, rhs: Exp) -> Exp.

> **Note**: The parameter names are optional, but recommended.

The types of the arguments and the term itself are easily derived from the constructor, and additionally the parameters have been given names.

The constructor can then be called to create a term:

    Plus(Int("4"), Int("5"))



## Relations and Rules
A relation describes how terms of one particular type can be rewritten into terms of another type. A relation, for example to evaluate an expression to a value, is defined as:

    def eval(e: Exp) -> Val.

> **Note**: The parameter names are optional, but recommended.

Next, one will define rules for implementing this relation. The rules match on certain term patterns and produce terms. For example:

    eval(Int(i)) -> IntV(i).

    eval(Plus(lhs, rhs)) -> v
    where eval(lhs) -> lv;
          eval(rhs) -> rv;
          add(lv, rv) -> v.

    // More rules here. For example add(Val, Val) -> Val.

> **Note**: The arrow `->` or `-name->` will invoke the function or relation only once. The double arrow `=>` or `=name=>` will invoke the function or relation as long as possible, until a final result is obtained.

By defining the relation, the Tego compiler can assert that the rules are exhaustive for the relation and the type of terms.

An alternative syntax is to specify the name of the relation on the arrow:

    Int(i) -eval-> IntV(i).

    Plus(lhs, rhs) -eval-> v
    where lhs -eval-> lv;
          rhs -eval-> rv;
          (lv, rv) -add-> v.

To invoke a relation, you prefix the arguments with the relation name:

    add(lv, rv) -> v

Or alternatively, you specify the name of the relation on the arrow:

    (lv, rv) -add-> v


## Functions
Sometimes it is needed to define functions to operate on data. For example, the `contains` function on a list has this signature and implementation:

    def contains<T>(list: List<T>, f: T -> Bool) -> Bool.

    contains([], _) -> False.
    contains([x|xs], f) -> r
    where
        if(f(x), True, contains(xs, f)) -> r.

> **Note**: This uses the special function `if` that's defined as:
>
>     def if<T>(c: Bool, ontrue: T, onfalse: T) -> T.
>     if(True,  t, _) -> t.
>     if(False, _, f) -> f.

> **Note**: The `Bool` type and the terms `True` and `False` are predefined.

> **Note**: This example uses the unqualified generic type `T`.

The function can be invoked using this syntax:

    contains(l, f) -> c

Or using the arrow syntax:

    f |- l -contains-> c

> **Note**: All arguments except the first go before the turnstile symbol `|-` when using arrow notation.

Or using the receiver syntax:

    l.contains(f) -> c



## Anonymous Functions
The previous example of `contains` expects a function `T -> Bool`. We can provide an existing function, for example:

    def always_true<T>(T) -> Bool.
    always_true(_) -> True.

    // In a rule:
    contains(l, always_true) -> c

Or we can create an anonymous function for which the types are inferred:

    contains(l, _ -> True) -> c


## Coercions
We can coerce a term into a function that creates that term by just specifying the term. For example:

    contains(-, True) -> c

Here `True` is a term that's coerced into an anonymous function:

    def anon0<T>(T) -> Bool.
    anon0(_) -> True.

> **Note**: The underscore `_` is not a valid identifier. It is used in places to specify an ignored argument.

Similarly, `Plus(Int("1"), Int("2"))` can be coerced into a function `T -> Exp`.



## Assignments and Arrows
You can assign using the arrow:

    e->v

> **Note**: You usuaully use this with the arrow reduction notation:
>
>     c |- e -eval-> v
>
> But you can use this with any expression:
>
>     eval(e, c) -> v
>

Or even use the assignment operator `:=`.

    v := eval(e, c)

> **Note**: You usually won't use this with the arrow reduction notation, but you can:
>
>    v := c |- e -eval

## Chaining
You can chain calls with the arrow notation:

    e -desugar-eval

> **Note**: You usually use this with the arrow reduction notation, which makes more sense syntactically:
>
>     e -desugar-eval-> v

But it can become unwieldy when you have arguments before the turnstile:

    c1 |- c2 |- e -desugar-eval

> **Note**: This is equivalent to:
>
>    c1 |- (c2 |- e -desugar) -eval

In which case it may be better to specify them on the arrow:

    e -desugar(c2)-eval(c1)

> **Note**: You usually use this with the arrow reduction notation, which makes more sense syntactically:
>
>     e -desugar(c2)-eval(c1)-> v

You can chain using the assignment arrow:

    eval(desugar(e, c2), c1)

And you can chain using the receiver notation:

    e.desugar(c2).eval(c1)


## Deconstruction
You can deconstruct with the arrow assignment. For example, the following rule succeeds only if the `desugar` relation succeeds and the result is successfully deconstructed.

    e -desugar-> BinOp(lhs, "+", rhs)

In the case of deconstructing tuples, the parenthesis are optional:

    E, S |- e -eval-> v, S'

Is equivalent to:

    E, S |- e -eval-> (v, S')

> **Note**: The apostrophe is a valid character in an identifier, allowing you to write `S'` and `S''` at will.

In the previous example `S'` was undefined, allowing it to be assigned a new value. However, when you use a variable that has already been assigned a value, you are essentially asserting that the result is exactly the same. For example, this reduction asserts that the resulting `S` in the tuple is exactly the same as the input `S`:

    E, S |- Bool(True) -eval-> v, S

> **Note**: The function `eval` has the signature:
>
>     def Env, Store |- Exp -eval-> Val, Store.
>
> Or, equivalently:
>
>     def eval(Exp, Env, Store) -> (Val, Store).


## Maps
You can get a value from a map using the indexer notation.

    v := E[n]

Which is syntactic sugar for:

    v := get(E, n)

You can set a value in a map:

    E' := set(E, n, v)

Or using syntactic sugar:

    E':= E ++ {n |-> v}

> **Note**: A map is a list or set or collection of pairs/assignments.
