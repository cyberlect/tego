# Functions

Function declaration

    def add(Y) X --> Z.

Function definition

    add(n): Succ(m) --> Succ(k)
    where m <add(n)> --> k.

---

Perhaps, if we can apply strategies by just putting their name after the argument?

    x inc

(Equivalent to `x <inc>` or `!x ; inc` in stratego.)

And we can leverage the simple arrow for assignment:

    x -> y

Then we can write the result of an application to a variable:

    x inc -> y

Or similarly:

    x inc-> y

Multiple applications chained together:

    x inc dec

And assigned:

    x inc dec -> y

---


Stratego stratgies:

    eq = ?(x, x)
    try(s) = s <+ id
    add(n) = ?Succ(m); where(!m ; add(m, n) ; ?k) ; !Succ(k)

Stratego rules:

    rename: VarDef(n, t) -> VarDef(n2, t)
    where n => n2

    add(n): Succ(m) -> Succ(k)
    where m <add(n)> => k.

DynSemSound arrows:

    add(X, Y) --> Z
    Y |- X -add-> Z

DynSemSound rules:

    add(Succ(m), n) --> Succ(k)
    where add(m, n) --> k.

    n |- Succ(m) -add-> Succ(k)
    where n |- m -add-> k.
