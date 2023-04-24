# Introduction to the Lambda Calculus #

We often talk about "*the* Lambda Calculus", as if there were just
one; but in fact, there are many, many variations.  The one we will
start with, and will explore in some detail, is often called "the pure"
or "the untyped" Lambda Calculus. Actually, there are many variations even under that heading. But all of the variations share a strong family
resemblance, so what we learn now will apply to all of them.

<details><summary>Fussy note (click to expand)</summary>
Calling this the "pure" Lambda Calculus is entrenched terminology,
but it coheres imperfectly with other uses of "pure" we'll encounter. There are
three respects in which the lambda calculus we'll be presenting might claim to
deserve the name "pure": (1) it has no pre-defined constants and a very spare
syntax; (2) it has no types; (3) it has no side-effects, and is insensitive to
the order of evaluation.

Sense (3) corresponds most closely to the other uses of "pure" you'll
see in the surrounding literature. With respect to this point, it may be true that the Lambda Calculus has no side effects. (Let's revisit that assumption at some point!) But as we'll see next week, it is *not* true that it's insensitive to the order of evaluation. So if that's what we mean by "pure", this lambda calculus isn't as pure as you might hope to get. Some *typed* lambda calculi will turn out to be more pure in that respect.

But this lambda calculus is at least "pure" in sense (2). At least, it
doesn't *explicitly talk about* any types. Some prefer to say that the
Lambda Calculus *does* have types implicitly, it's just that
there's only one type, so that every expression is a member of
that one type. If you say that, you have to say that functions from
this type to this type also belong to this type. Which is weird... In
fact, though, such types are studied, under the name "recursive
types." More about these later.

Well, at least this lambda calculus is "pure" in sense (1). As we'll
see when we study combinatorial logic, though, there are some computational formal systems
whose syntax is *even more* spare, in that they don't even have variables.
So if that's what mean by "pure", again this lambda calculus isn't as pure
as you might hope to get.

Still, if you say you're talking about "the pure" Lambda Calculus,
or "the untyped" Lambda Calculus, or even just "the" Lambda Calculus, this
is the system that people will understand you to be referring to.
</details>

## Syntax ##

Here is its syntax:

<blockquote>
<strong>Variables</strong>: <code>x</code>, <code>y</code>, <code>z</code> ...
<br>  
<strong>Abstract</strong>: <code>(&lambda;a M)</code>
<br>
<strong>Application</strong>: <code>(M N)</code>
</blockquote>

Unpacking the syntax: each variable is an expression. For any variable `a` and (possibly complex) expressions `M` and `N`, 
<code>(&lambda;a M)</code> and <code>(M N)</code> are also expressions.
That's it!  A formal system capable of expressing any computable function, in the space of three lines.

We'll tend to write <code>(&lambda;a M)</code> as just `(\a M)`, so we
don't have to write out the markup code for the
<code>&lambda;</code>. You can yourself write <code>(&lambda;a
M)</code> or `(\a M)` or `(lambda a M)`.

Expressions in the lambda calculus are called "terms".  Here is the
syntax of the lambda calculus given in the form of a context-free
grammar:

> T --> Var  
> T --> ( &lambda; Var T)  
> T --> ( T T )  
> Var --> x  
> Var --> y  
> Var --> z  
> ...

Very, very simple.

Sometimes the first two production rules are further distinguished, and those
are called more specifically "value terms". Whereas Applications (terms of the
form `(M N)`) are terms but not value terms.

Examples of terms:

    x
    (y x)
    (x x)
    (\x y)
    (\x x)
    (\x (\y x))
    (x (\x x))
    ((\x (x x)) (\x (x x)))


## Beta-Reduction ##

The lambda calculus has an associated proof theory. For now, we can regard the
proof theory as having just one rule, called the rule of **beta-reduction** or
"beta-contraction". Suppose you have some expression of the form:

    ((\a M) N)

This is an application whose first element is an abstract.  This
compound form is called a **redex**, meaning it's a "beta-reducible
expression." `(\a M)` is called the **head** of the redex; `N` is
called the **argument**, and `M` is called the **body**.

The rule of beta-reduction permits a transition from that expression to the following:

> <code>M</code> [<code>a</code> <-- <code>N</code>]

What this means is just `M`, with any *free occurrences* inside `M` of
the variable `a` replaced with the term `N` (--- "without capture", which
we'll explain in the [[advanced notes|week2_lambda_advanced]]).

What is a free occurrence?

> Any occurrence of a variable `a` is **bound** in T when T has the form `(\a N)`.

> An occurrence of a variable `a` is **bound** in T when T has the form `(\b
N)` --- that is, with a *different* variable `b` --- just in case that
occurrence of `a` is bound in the subexpression `N`.

> When T has the form `(M N)`, any occurrences of `a` that are bound in the
subexpression `M` are also bound in T, and so too any occurrences of `a` that
are bound in the subexpression `N`.

> An occurrence of a variable is **free** if it's not bound.

For instance consider the following term `T`:

    (x (\x (\y (x (y z)))))

The first occurrence of `x` in T is free.  The `\x` we won't regard as
containing an occurrence of `x`. The next occurrence of `x` occurs
within a form `(\x (\y ...))` that begins with `\x`, so it is bound as well. The
occurrence of `y` is bound; and the occurrence of `z` is free.

To read further:

* [[!wikipedia Free variables and bound variables]]

Here's an example of beta-reduction:

    ((\x (y x)) z)

beta-reduces to:

    (y z)

We'll write that like this:

    ((\x (y x)) z) ~~> (y z)

Different authors use different terminology and notation. Some authors use the term
"contraction" for a single reduction step, and reserve the term
"reduction" for the reflexive transitive closure of that, that is, for
zero or more reduction steps. Informally, it seems easiest to us to
say "reduction" for one or more reduction steps. So when we write:

    M ~~> N

we'll mean that you can get from `M` to `N` by one or more reduction
steps.

When `M` and `N` are such that there is some common term (perhaps just one
of those two, or perhaps a third term) that they both reduce to,
we'll say that `M` and `N` are **beta-convertible**. We'll write that
like this:

    M <~~> N

More details about the notation and metatheory of
the lambda calculus are in [[this week's advanced notes|topics/week2_lambda_advanced]].


## Shorthand ##

The grammar we gave for the lambda calculus leads to some
verbosity. There are several informal conventions in widespread use,
which enable the language to be written more compactly. (If you like,
you could instead articulate a formal grammar which incorporates these
additional conventions. Instead of showing it to you, we'll leave it
as an exercise for those so inclined.)


**Parentheses** Outermost parentheses around applications can be dropped. Moreover, applications will associate to the left, so `M N P` will be understood as `((M N) P)`. Finally, you can drop parentheses around abstracts, but not when they're part of an application. So you can abbreviate:

    (\x (x y))

as:

    \x (x y)

but you should include the parentheses in:

    (\x (x y)) z

and:

    z (\x (x y))


**Dot notation** Dot means "Insert a left parenthesis here, and the matching right parenthesis as far to the right as possible without creating unbalanced parentheses---so long as doing so would enclose an application or abstract not already wrapped in parentheses." Thus:

    \x (\y (x y))

can be abbreviated as:

    \x (\y. x y)

and that as:

    \x. \y. x y

This:

    \x. \y. (x y) x

abbreviates:

    \x (\y ((x y) x))

This on the other hand:

    (\x. \y. (x y)) x

abbreviates:

    ((\x (\y (x y))) x)

The outermost parentheses were added because we have an application. `(\x. \y.
...)` became `(\x (\y. ...))` because of the rule for dots. We didn't
insert any parentheses around the inner body of `\y. (x y)` because they were
already there. That is, in expressions of the form `\y. (...)`, the dot abbreviates
nothing. It's harmless to write such a dot, though, and it can be conceptually
helpful especially in light of the next convention.

Similarly, we permit `\x. x`, which is shorthand for `\x x`, not for `\x (x)`, which
our syntax forbids. (The [[lambda evaluator|/code/lambda_evaluator]] however tolerates such expressions.)


**Merging lambdas** An expression of the form `(\x (\y M))`, or equivalently, `(\x. \y. M)`, can be abbreviated as:

    (\x y. M)

Similarly, `(\x (\y (\z M)))` can be abbreviated as:

    (\x y z. M)


## Lambda terms represent functions ##

Let's pause and consider the following fundamental question: what is a
function?  One popular answer is that a function can be represented by
a set of ordered pairs.  This set is called the **graph** of the
function.  If the ordered pair `(a, b)` is a member of the graph of `f`,
that means that `f` maps the argument `a` to the value `b`.  In
symbols, `f: a` &mapsto; `b`, or `f (a) == b`.

In order to count as a *function* (rather
than as merely a more general *relation*), we require that the graph not contain two
(distinct) ordered pairs with the same first element.  That is, in
order to count as a proper function, each argument must correspond to
a unique result.

The lambda calculus seems to be wonderfully well-suited for
representing functions.  In fact, the untyped
lambda calculus is Turing Complete (see [[!wikipedia Turing completeness]]):
all (recursively computable) functions can be represented by lambda
terms. Which, by most people's lights, means that all functions we can "effectively decide" ---
that is, always apply in a mechanical way without requiring any ingenuity or insight, and be guaranteed of a correct answer after some finite number of steps ---
can be represented by lambda terms. As we'll see, though, it will be fun
(that is, not straightforward) unpacking how these things can be so "represented."

For some lambda terms, it is easy to see what function they represent:

> `(\x x)` represents the identity function: given any argument `M`, this function
simply returns `M`: `((\x x) M) ~~> M`.

> `(\x (x x))` duplicates its argument (applies it to itself):
`((\x (x x)) M) ~~> (M M)` <!-- **M** or &omega;; W is \uv.uvv, L is \uv.u(vv) -->

> `(\x (\y (y x)))` reorders its two arguments:
`(((\x  (\y (y x))) M) N) ~~> (N M)` <!-- **T**; C is \uvx.uxv -->

> `(\x (\y x))` throws away its second argument:
`(((\x (\y x)) M) N) ~~> M` <!-- **K** -->

and so on.  In order to get an intuitive feel for the power of the
lambda calculus, note that duplicating, reordering, and deleting
elements is all that it takes to simulate the behavior of a general
word processing program.  That means that if we had a big enough
lambda term, it could take a representation of *Emma* as input and
produce *Hamlet* as a result.

Some of these functions are so useful that we'll give them special
names.  In particular, we'll call the identity function `(\x x)`
**I**, and the function `(\x (\y x))` **K** (for "konstant": <code><b>K</b> x</code> is
a "constant function" that accepts any second argument `y` and ignores
it, always returning `x`).

It is easy to see that distinct lambda expressions can represent the same
function, considered as a mapping from input to outputs. Obviously:

    (\x x)

and:

    (\z z)

both represent the same function, the identity function. However, we said
[[in the advanced notes|week2_lambda_advanced]] that we would be regarding these expressions as
synactically equivalent, so they aren't yet really examples of *distinct*
lambda expressions representing a single function. However, all three of these
are distinct lambda expressions:

    (\y x. y x) (\z z)

    (\x. (\z z) x)

    (\z z)

yet when applied to any argument `M`, all of these will always return `M`. So they
have the same extension. It's also true, though you may not yet be in a
position to see, that no other function can differentiate between them when
they're supplied as an argument to it. However, these expressions are all
syntactically distinct.

The first two expressions are (beta-)*convertible*: in particular the first
reduces to the second via a single instance of beta reduction. So they
can be regarded as proof-theoretically equivalent even though they're
not syntactically identical. However, the proof theory we've given so
far doesn't permit you to reduce the second expression to the
third. So these lambda expressions are non-equivalent.

In other words, we have here different (non-equivalent) lambda terms with the
same extension. This introduces some tension between the popular way of
thinking of functions as represented by (identical to?) their graphs or
extensions, and the idea that lambda terms express functions. Well, perhaps the
lambda terms are just a finer-grained way of expressing functions than
extensions are?

As we'll see, though, there are even further sources of
tension between the idea of functions-as-extensions and the idea of functions
embodied in the lambda calculus.


1.  One reason is that that general
mathematical conception permits many *uncomputable* functions, but the
lambda calculus can't express those.

2.  More problematically, lambda terms express "functions" that can *take themselves* as arguments.  If we wanted to represent that set-theoretically, and
identified functions with their extensions, then we'd have to have some
extension that contained (an ordered pair containing) itself as a member. Which
we're not allowed to do in mainstream set-theory.  But in the lambda calculus
this is permitted and common --- and in fact will turn out to be indispensable.

    Here are some simple examples. We can apply the identity function to itself:

        (\x x) (\x x)

   This is a redex that reduces to the identity function (of course).

   We can apply the **K** function to another argument and itself:

   > <code><b>K</b> z <b>K</b></code>

    That is:

        (\x (\y x)) z (\x (\y x))

    That reduces to just `z`.

3.  As we'll see in coming weeks, some lambda terms will turn out to be
impossible to associate with any extension. This is related to the previous point.


In fact it *does* turn out to be possible to represent the Lambda Calculus
set-theoretically. But not in the straightforward way that identifies
functions with their graphs. For years, it wasn't known whether it would be possible to do this. But then
[[!wikipedia Dana Scott]] figured out how to do it in late 1969, that is,
he formulated the first "denotational semantics" for this Lambda Calculus.
Scott himself had expected that this wouldn't be possible to do. He argued
for its unlikelihood in a paper he wrote only a month before the discovery.

(You can find an exposition of Scott's semantics in Chapters 15 and 16 of the
Hindley &amp; Seldin book we recommended, and an exposition of some simpler
models that have since been discovered in Chapter 5 of the Hankin book, and
Section 16F of Hindley &amp; Seldin. But realistically, you really ought to
wait a while and get more familiar with these systems before you'll be at all
ready to engage with those ideas.)

We've characterized the rule of beta-reduction as a kind of "proof theory" for
the Lambda Calculus. In fact, the usual proof theory is expressed in terms of
*convertibility* rather than in terms of reduction; but it's natural to
understand reduction as being the conceptually more fundamental notion. And
there are some proof theories for this system that are expressed in terms of
reduction.

To use a linguistic analogy, you can think of what we're calling
"proof theory" as a kind of syntactic transformation. The
sentences *John saw Mary* and *Mary was seen by John* are not
syntactically identical, yet (on some theories) one can be derived
from the other. The key element in the analogy is that this kind of
syntactic derivation is supposed to preserve meaning, so that the two
sentences mean (roughly) the same thing.

There's an extension of the proof-theory we've presented so far which
does permit a further move of just that sort. It would permit us to
also count these functions:

    (\x. (\z z) x)

    (\z z)

as equivalent. This additional move is called **eta-reduction**. It's
crucial to eta-reduction that the outermost variable binding in the
abstract we begin with (here, `\x`) be of a variable that occurs free
exactly once in the body of that abstract, and that that free occurrence be the rightmost outermost constituent.

The expression:

    (\x (\y (y x))

can't be eta-reduced, because the rightmost outermost constituent is not `x` but `(\y (y x)`.

In the extended proof theory/theories we get be permitting eta-reduction/conversion as well as beta-reduction, *all computable functions with the same
extension do turn out to be equivalent*, that is, convertible.

However, we still shouldn't assume we're working with functions
traditionally conceived as just sets of ordered pairs, for the other
reasons sketched above.


## The analogy with `let` ##

In our basic functional programming language, we used `let`
expressions to assign values to variables.  For instance,

    let x match 2
    in (x, x)

evaluates to the ordered pair `(2, 2)`.  It may be helpful to think of
a redex in the lambda calculus as a particular sort of `let`
construction.

    ((\x BODY) ARG)

is analogous to

    let x match ARG
    in BODY

This analogy should be treated with caution.  For one thing, our `letrec`
allowed us to define recursive functions in which the name `x` appeared within
`ARG`, but we wanted it there to be bound to the very same `ARG`.  We're not
ready to deal with recursive functions in the lambda calculus yet.

For another, we defined `let` in terms of *values*: we said that the variable
`x` was bound to whatever *value* `ARG` *evaluated to*. At this point, it's
not clear what the values are in the lambda calculus. We only have expressions.
(Though there was a suggestive remark earlier about some of the expressions but
not others being called "value terms".)

So perhaps we should translate `((\x BODY) ARG)` in purely syntactic terms,
like: "let `x` be replaced by `ARG` in `BODY`"?

Most problematically, in the lambda
calculus, an abstract such as `(\x (x x))` is perfectly well-formed
and coherent, but it is not possible to write a `let` expression that
does not have an `ARG`. That would be like:

&nbsp;&nbsp;&nbsp;&nbsp;`let x match` *missing*  
&nbsp;&nbsp;&nbsp;&nbsp;`in x x`

Nevertheless, the correspondence is close enough that it can guide our
intuition.

