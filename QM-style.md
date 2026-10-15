# AP40025 QM Authoring Conventions

These course-specific conventions supplement the shared conventions in
[rs-notes-conv.md](rs-notes-conv.md)

## Preserving homework calculations

Keep the user's correct intermediate homework steps, including calculations
that could be shortened or omitted. Add the next step and make local
corrections without replacing the derivation with a condensed proof. When the
user chooses a different method, superseded intermediate steps may be removed

## QM perturbation-order notation

Parenthesized superscripts denote perturbation order.
Use `\psi_n^{\porder{k}}` and `E_n^{\porder{k}}` for perturbation-order
coefficients. The `\porder` macro renders parentheses and links to
`quantum-mechanics::ch07::conv:order-k-correction`. The same parenthesized order
marks a truncated perturbative approximation. Define this notation in a
`conv` block; do not use it for differentiation

Use `\DiffOp^k(f)` for differentiation and `\DiffOp^{-1}(f)` for a
chosen antiderivative. The `\DiffOp` macro renders upright D in prerequisite
purple until its definition is available. An antiderivative is unique only
after fixing its additive constant; endpoint differences do not depend on it
