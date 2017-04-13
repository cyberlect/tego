# Rules and Relations


## Relation
Here is a relation:

    R : A * B -> 2


## Judgement
Here is a judgement with a signature `s`:

    C |-(s) x : t

You can combine multiple judgements using the `or` operator:

    C |- x or C |- y

You can combine multiple judgements using the `,` (and) operator:

    C |- x, C |- y
