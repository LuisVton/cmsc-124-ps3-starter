# Reasoning for the Predictions Table

Write one entry under every heading below, then commit this file together with
`predictions.tsv`, before you run a case or open `tests/expected.tsv`.

Each entry names the rule that produces the four predicted fields for that
case. One to three sentences is enough. The grader checks structure only. It
does not judge quality, and it sets no word minimum.

After you run the cases, add a short correction under each case you predicted
wrongly. Say what you predicted, what CLISP printed, and which rule you
misread. Those entries belong in the correction commits.

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
Replace this line with the rule behind this case's four fields.

## P10
Replace this line with the rule behind this case's four fields.

## P11
Replace this line with the rule behind this case's four fields.

## P12
Replace this line with the rule behind this case's four fields.

## P13
Replace this line with the rule behind this case's four fields.

## P14
Replace this line with the rule behind this case's four fields.

## P15
Replace this line with the rule behind this case's four fields.

## P16
Replace this line with the rule behind this case's four fields.
