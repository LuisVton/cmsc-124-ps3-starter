# Environments and the Toy Evaluator

Replace this paragraph after your checks pass. Trace one form through your own
`eval-expr`, point to the line that captures the defining environment, say what
would change if application joined the arguments to the caller's environment
instead, and explain why the host CLISP needs `funcall` where the toy language
doesn't.

# Analysis

Consider the following expression evaluated in an empty environment:

```lisp
(let ((x 2))
  (let ((f (lambda (y) (+ x y))))
    (let ((x 100))
      (f 1))))
```

The expression demonstrates how the interpreter implements lexical scoping through closures. The `eval-expr` function delegates each `let` expression to `eval-let`, which evaluates its initializers before extending the environment. The first binding assigns `x` the value `2`. When the lambda expression is evaluated, it becomes a closure that preserves the environment in which it was created. In `src/evaluator.lisp`, line 83, `(list :closure params body env)` stores the closure's parameters, body, and defining environment.

Although the innermost `let` introduces another binding for `x` with the value `100`, calling `(f 1)` still uses the environment captured by the closure. The `apply-closure` function extends that saved environment with the parameter binding `(y . 1)`. Consequently, evaluating `(+ x y)` looks up `x` as `2` and `y` as `1`, producing `3`. If the interpreter instead used the caller's environment during function application, it would find the inner `x` binding and return `101`. That behavior would demonstrate dynamic rather than lexical scoping.

The toy language also follows a Lisp-1 design, meaning variables and functions share one namespace. This differs from Common Lisp, which uses separate namespaces for variables and functions. In the host language, a function stored in a variable is invoked using `funcall`, as in `(funcall f 1)`. In the toy interpreter, `eval-expr` evaluates the operator expression using the same environment as other expressions, allowing `(f 1)` to retrieve and apply the closure directly.

Environments are represented as association lists, with newer bindings placed at the front. For the closure body in this example, the environment begins with `((y . 1) (x . 2))`. The `lookup` function returns the first matching binding, allowing inner bindings to shadow outer ones. It checks whether a binding exists rather than whether its value is truthy, since a variable may legitimately be bound to `nil`. An unbound variable signals an error instead of silently returning a value.

The `extend-env` function adds new bindings without modifying the existing environment. This preserves surrounding bindings when evaluation leaves a `let` expression. Additionally, `eval-let` evaluates all initializers in the original environment before extending it. Thus, `(let ((x 10) (y x)) y)` returns the previous value of `x`, because the new binding for `x` is not visible while evaluating `y`.

Finally, `apply-closure` validates the closure representation and checks that the number of supplied arguments matches the number of parameters before evaluating the body. Together, these design choices support arithmetic evaluation, variable lookup, parallel `let` bindings, and lexical scoping, including function calls through variables and closures passed as arguments.