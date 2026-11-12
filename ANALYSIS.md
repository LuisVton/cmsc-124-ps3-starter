# Environments and the Toy Evaluator

Consider this form, evaluated with an empty environment:

```lisp
(let ((x 2))
  (let ((f (lambda (y) (+ x y))))
    (let ((x 100))
      (f 1))))
```

`eval-expr` sends each `let` to `eval-let`, which evaluates its initializers
before extending the environment. The first binding gives `x` the value 2,
then the lambda becomes a closure that saves this environment. In
`src/evaluator.lisp`, line 83, `(list :closure params body env)` captures it.

The inner `let` binds `x` to 100, but `(f 1)` still finds the earlier closure.
`apply-closure` adds `(y . 1)` to its saved environment, so evaluating `(+ x y)`
looks up 2 and 1 and returns 3. If application used the caller's environment
instead, it would find 100 and return 101, which would be dynamic scope.

Host CLISP has separate namespaces for variables and functions, so a function
stored in variable `f` is called with `(funcall f 1)`. The toy language uses
one namespace, and `eval-expr` evaluates the operator expression through that
same environment, so `(f 1)` directly finds and applies the closure.

The environment is an association list, with the newest bindings at the front.
For the closure body above, it starts as `((y . 1) (x . 2))`. `lookup` uses
the first matching entry, which lets an inner binding shadow an outer one.
It also checks whether the binding exists rather than whether its value is
true, since a variable could be bound to `nil`. An absent name signals an
error instead of silently producing a value.

`extend-env` adds fresh pairs without changing the existing environment, so
leaving a `let` body does not change the surrounding bindings. Also, all
initializers use the same original environment. For example, if the outer
`x` is 2, then `(let ((x 10) (y x)) y)` returns 2 because `y` cannot see
the new `x` yet. This is why `eval-let` evaluates the initializers before
calling `extend-env`.

Finally, `apply-closure` checks the closure tag and argument count before
evaluating the body. The public checks pass for arithmetic, lookup, parallel
bindings, and lexical scope, including calls through variables and closures
passed as arguments.
