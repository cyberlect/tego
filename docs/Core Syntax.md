# Core Syntax


## Patterns
A pattern has the following form, where `v` is an unbound variable and `T` is a term with patterns as its arguments:

    v@T

If `T` is a term, it matches its corresponding term and recurses to its arguments. If `T` is an underscore `_`, it matches anything. If `T` is a bound variable, it matches the exact term in the variable. If `T` is an unbound variable, that's an error.

If `v` is an unbound variable, the term that matched to `T` is stored in it. If `v` is a bound variable, that's an error. If `v` is an underscore `_`, nothing is stored.

> **Note**: The following syntactic sugar is available:
>
> - Matching a term `T` (instead of `_@T`).
> - Matching a variable `v` (instead of `_@v`).
> - Binding a variable `v` (instead of `v@_`).
>
> Note that the syntax `v` can mean binding or matching depending on whether `v` is bound or not.

Pattern variables use static scoping. That is, a pattern can use variables defined in the same lexical scope, and define new variables in the same lexical scope.


## Matching
The special syntax built-in function `?(Term, Pattern) -> Term` (match) will match the term to the pattern, match any bound variables, assign any unbound variables, and return the input term. Most Tego operations are desugared to this special `?` (match) function, and it can also be used to create functions that succeed only on a particular match.

> **Note**: `Term` is the super type of all terms. `Pattern` is not actually a type you can use.

> **Fixme**: If a pattern binding variables is passed down somewhere (e.g. `?` in a closure), and it either binds down below or maybe is even never executed, then what happens to the (still unbound) variables at the call site?




## Rules
A rule takes a term pattern `P` and produces a term `t`, with an optional where clause `c`.

    P -> t
    where c.

The rule will fail if the pattern `P` doesn't match or the clause `c` fails.


## Arrow Assignment/Deconstruction
The arrow assignment takes a term `t` and assigns it to a deconstruction pattern `P`.

    t -> P

If the term pattern contains unbound variables, they will be bound. If the pattern contains bound variables, they will be matched.
