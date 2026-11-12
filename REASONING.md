# Reasoning for the Predictions Table

## P01
Multiplication gives 6, then addition gives 7, so the result is an integer with
no sequence length. The outer operator `+` is a function.

## P02
The quote returns `(+ 1 2)` as data, so the result is a cons with three elements,
written `(+,1,2)` in the table. The outer operator is `quote`, a special operator.

## P03
`nil` evaluates to itself and is also the empty list, so its course kind is
`null` and its length is 0. The form is an atom, so its operator is `none`.

## P04
`t` evaluates to itself, so the result is a symbol with no sequence length.
Since the form is an atom, its operator is `none`.

## P05
`nil` is a symbol as well as an empty list, so `symbolp` returns `t`, a symbol
with no sequence length. The outer operator `symbolp` is a function.

## P06
Taking `car` of the empty list returns `nil`, so the kind is `null` and the
length is 0. The outer operator `car` is a function.

## P07
`cons` puts 1 before `(2 3)`, giving `(1,2,3)`, a cons with three elements.
The outer operator `cons` is a function.

## P08
The empty list satisfies `null`, so `if` selects the quoted symbol `empty`.
The result has no sequence length, and `if` is a special operator.

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
