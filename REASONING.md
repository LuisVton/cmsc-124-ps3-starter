# Reasoning for the Predictions Table

## P01
`+` is a function, so it evaluates the nested multiplication first (2 × 3 = 6) and adds 1 to obtain 7. The result is an integer, not a sequence, so its length is `none`.

## P02
The reader expands the leading quote to `(quote (+ 1 2))`; `quote` is a special operator and does not evaluate the addition. Its result is the three-element list `(+ 1 2)`, a nonempty cons-based list.

## P03
`nil` is an atomic form that evaluates to itself, so it has no operator. Under the course classification it is `null` (not `symbol`), and as the empty sequence it has length 0.

## P04
`t` evaluates to itself and is a non-`nil` symbol, not a sequence. As an atomic form it has no head operator and its length is `none`.

## P05
`symbolp` is a function and evaluates its argument `nil` before testing it. Since `nil` is also a symbol, the result is `t`, which the course calls a `symbol` with no sequence length.

## P06
`car` is a function; the quoted empty list supplies `nil` without evaluating its contents. `car` of the empty list returns `nil`, which is `null` and has sequence length 0.

## P07
`cons` is a function that evaluates its arguments, then puts 1 in front of the quoted list `(2 3)`. The result `(1 2 3)` is a nonempty list (`cons`) with three top-level elements.

## P08
`if` is a special operator that evaluates the condition and only the selected branch. `(null '())` is true, so the quoted symbol `empty` is returned; it is a symbol with no sequence length.

## P09
`and` stops at `nil` and returns it, so the kind is `null` and the length is 0.
The course checks special operators first, so CLISP's `and` is classified as
`special-operator`.

## P10
`defvar` makes `*ps3-rate*` special, so the temporary binding of 10 is used
inside `ps3-scale`, giving 30. This is an integer with no sequence length,
and the outer `progn` is a special operator.

## P11
`defun` defines the function and returns its name, `ps3-twice`, so the result
is a symbol with no sequence length. The outer operator `defun` is a macro.

## P12
`labels` allows `down` to call itself, so it builds `(3,2,1)` before reaching
the empty list at 0. The result is a cons of length 3, and `labels` is a
special operator.

## P13
`mapcar` applies `length` to each sublist, giving `(2,0,1)`, a cons of length 3.
The outer operator `mapcar` is a function.

## P14
The lambda squares each number through `mapcar`, giving `(1,4,9)`, a cons of
length 3. The outer operator `mapcar` is a function.

## P15
Evaluating the lambda produces a function, written `#<function>`, with no
sequence length. The outer operator `lambda` is a macro.

## P16
The closure captures `factor` as 2, so the later binding of 10 does not change
its multiplication, which gives 6. The result is an integer with no sequence
length, and the outer `let` is a special operator.
