# Environments and the Toy Evaluator

<<<<<<< HEAD
Consider this form, evaluated with an empty environment:
=======
Consider the following expression evaluated in an empty environment:
>>>>>>> 767611b80d925de1023815bc0a134c74f7c64bc4

```lisp
(let ((x 2))
  (let ((f (lambda (y) (+ x y))))
    (let ((x 100))
      (f 1))))
```

<<<<<<< HEAD
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
=======
`eval-expr` sends each `let` to `eval-let`, which evaluates its initializers before extending the environment. The first binding gives `x` the value 2. The lambda then becomes a closure that saves this environment. In `src/evaluator.lisp`, line 82, `(list :closure params body env)` stores the closure's parameters, body, and defining environment.

The inner `let` binds `x` to 100, but `(f 1)` still uses the closure's saved environment. The `apply-closure` function extends that saved environment with the parameter binding `(y . 1)`. Consequently, evaluating `(+ x y)` looks up `x` as `2` and `y` as `1`, producing `3`. If the interpreter instead used the caller's environment during function application, it would find the inner `x` binding and return `101`. That behavior would demonstrate dynamic rather than lexical scoping.

The toy language is a Lisp-1, so variables and functions share one namespace. Host Common Lisp separates these namespaces, so a function stored in a variable needs `funcall`, as in `(funcall f 1)`. In the toy interpreter, `eval-expr` evaluates the operator expression using the same environment as other expressions, allowing `(f 1)` to retrieve and apply the closure directly.

Environments are represented as association lists, with newer bindings placed at the front. For the closure body in this example, the environment begins with `((y . 1) (x . 2))`. The `lookup` function returns the first matching binding, allowing inner bindings to shadow outer ones. It checks whether a binding exists rather than whether its value is truthy, since a variable may legitimately be bound to `nil`. An unbound variable signals an error instead of silently returning a value.

The `extend-env` function adds new bindings without modifying the existing environment. This preserves surrounding bindings when evaluation leaves a `let` expression. Additionally, `eval-let` evaluates all initializers in the original environment before extending it. Thus, `(let ((x 10) (y x)) y)` returns the previous value of `x`, because the new binding for `x` is not visible while evaluating `y`.

Finally, `apply-closure` checks the closure tag and argument count before evaluating the body. Together, these choices support parallel `let` bindings and lexical scope, including calls through variables and closures passed as arguments.
>>>>>>> 767611b80d925de1023815bc0a134c74f7c64bc4
