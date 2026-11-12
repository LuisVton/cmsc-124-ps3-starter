# Environments and the Toy Evaluator

Consider the following expression evaluated in an empty environment:

```lisp
(let ((x 2))
  (let ((f (lambda (y) (+ x y))))
    (let ((x 100))
      (f 1))))
```

`eval-expr` sends each `let` to `eval-let`, which evaluates its initializers before extending the environment. The first binding gives `x` the value 2. The lambda then becomes a closure that saves this environment. In `src/evaluator.lisp`, line 82, `(list :closure params body env)` stores the closure's parameters, body, and defining environment.

The inner `let` binds `x` to 100, but `(f 1)` still uses the closure's saved environment. The `apply-closure` function extends that saved environment with the parameter binding `(y . 1)`. Consequently, evaluating `(+ x y)` looks up `x` as `2` and `y` as `1`, producing `3`. If the interpreter instead used the caller's environment during function application, it would find the inner `x` binding and return `101`. That behavior would demonstrate dynamic rather than lexical scoping.

The toy language is a Lisp-1, so variables and functions share one namespace. Host Common Lisp separates these namespaces, so a function stored in a variable needs `funcall`, as in `(funcall f 1)`. In the toy interpreter, `eval-expr` evaluates the operator expression using the same environment as other expressions, allowing `(f 1)` to retrieve and apply the closure directly.

Environments are represented as association lists, with newer bindings placed at the front. For the closure body in this example, the environment begins with `((y . 1) (x . 2))`. The `lookup` function returns the first matching binding, allowing inner bindings to shadow outer ones. It checks whether a binding exists rather than whether its value is truthy, since a variable may legitimately be bound to `nil`. An unbound variable signals an error instead of silently returning a value.

The `extend-env` function adds new bindings without modifying the existing environment. This preserves surrounding bindings when evaluation leaves a `let` expression. Additionally, `eval-let` evaluates all initializers in the original environment before extending it. Thus, `(let ((x 10) (y x)) y)` returns the previous value of `x`, because the new binding for `x` is not visible while evaluating `y`.

Finally, `apply-closure` checks the closure tag and argument count before evaluating the body. Together, these choices support parallel `let` bindings and lexical scope, including calls through variables and closures passed as arguments.
