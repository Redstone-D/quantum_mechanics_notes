---
name: rs-notes
description: Rua's personal conventions for authoring mathematical notes with the Academy Notes Toolchain. Covers function-definition style (Lean 4 type-theoretic), syntactic sugar via `\conv` blocks, equality symbols (`\defeq` as `\equiv`), the partial-operator three-piece pattern, proof formatting, macro linking rules, and juxtaposition conventions. Activate when editing `.tex` or `.sty` files in a math-notes project that uses `notes-common.sty` / `notes-project.sty`, or when the user mentions "my notes" / "rs-notes" / "math notes authoring".
---

# Rua's Math Notes Authoring Conventions

Personal cross-project conventions for mathematical notes. Scope:
- **Covers:** authoring style, notation choices, macro-link routing, proof formatting.
- **Does not cover:** toolchain mechanics (see `academy-notes-toolchain` skill), per-course identity (`notes-project.sty`), generic LaTeX typesetting.

## LaTeX library: `rs-notes.sty`

Projects using these conventions should load the companion package `rs-notes.sty` (shipped with this skill). It defines the style-specific macros:

- `\defeq` — renders as `\equiv`, used for definitional/notational equality.
- (Add more as the style evolves.)

Install into a course repo by copying `rs-notes.sty` next to `notes-project.sty`, then adding `\RequirePackage{rs-notes}` to `notes-project.sty`.

## Conventions

### 1. Function definitions (Lean 4 / type-theoretic style)

Format: `name : type \defeq term` on one line (if it fits).

```latex
\mathrm{abs} : F \to F \defeq a \map
\begin{cases}
  a,  & a \geq 0, \\
  -a, & a < 0.
\end{cases}
```

- Use `\map` for the rule (function-object style).
- **Multi-argument** functions may use the **product type** if clearer: `f : A \times B \to C,\; (a, b) \map \ldots`
- Usually no currying unless partial application is actually used.
- Function identity is `\mathrm{name}` (upright): `\mathrm{abs}`, `\mathrm{dist}`, `\mathrm{nbhd}`.

### 2. Syntactic sugar lives in `\conv`, not `\dfn`

- `\begin{dfn}` defines the mathematical **function/object** — *what it IS*.
- `\begin{conv}` defines the **notation sugar** — *how we WRITE it*.
- Pattern:
  ```latex
  \begin{dfn}[Absolute Value]{absolute-value}
    ... \mathrm{abs} : F \to F \defeq a \map \ldots
  \end{dfn}

  \begin{conv}[Absolute Value Notation]{absolute-value-notation}
    For readability, we write $|a|$ in place of $\mathrm{abs}(a)$, where
    $\mathrm{abs}$ is defined in \xref{def:absolute-value}.
  \end{conv}
  ```
- A reader clicking the sugared form lands on the convention; one more click reaches the function definition.

### 3. Equality symbols

| Symbol | Meaning | Example |
|---|---|---|
| `=` | Value equality | `2 + 2 = 4` |
| `\equiv` (via `\defeq` macro) | Definition / notational equivalence | `d(x, y) \equiv \lvert x - y \rvert` |
| `\iff` / `\Leftrightarrow` | Logical equivalence | `a = b \iff \forall \varepsilon > 0, |a - b| < \varepsilon` |

No `:=` or `\coloneqq` — replaced by `\defeq` (which renders as `\equiv`).

### 4. Partial operators (three-piece pattern)

Any operator defined only on a restricted domain gets three pieces:

1. **Predicate / concept** — e.g., `def:limit` (binary: "the sequence has a limit of ℓ").
2. **Uniqueness theorem** — e.g., `thm:limit-uniqueness`.
3. **Operator with subset-type domain** — e.g., `def:limit-op`: `\mathrm{Conv}(F) \to F`.
4. **Well-definedness remark** — two bullets:
   - *Every input has an output* (existence from the predicate def).
   - *Every input has exactly one output* (uniqueness from the theorem).

Applied uniformly to `\seqlim`, `\sup`, `\inf`, and any future partial operators.

### 5. Quantifier style

- Restricted type goes **inside** the type: `\forall \varepsilon \in F_{>0}` (not `\forall \varepsilon \in F, \varepsilon > 0`).
- Dependent bounds **stay separate**: `\forall n \in \NN_1,\, n \geq N` (not folded into type).
- Thin space `\,` between quantifier restrictions.

### 6. Proofs

- Two-direction (iff) proofs: `\noindent\textbf{($\Rightarrow$)}` and `\noindent\textbf{($\Leftarrow$)}` headers.
- **One QED per proof** — let `\end{proof}` auto-insert it. No manual `\qed`.
- Each direction should fit in 1–3 sentences with display math for the key equation chain.
- **Rename bound variables** to avoid collision (e.g., use `\gamma` for an internal variable when `\varepsilon` is the goal's variable).
- **Avoid citing section/theorem numbers** in `\xref` prose — use descriptive text: `\xref[definition of convergent sequence]{def:convergent-sequence}`, not `\xref[Dfn 1.1]{...}`.

### 7. Macros for linked notation

Live in the per-project notation package (e.g., `real-analysis-notations.sty`).

**Rule:** The following must be linked via a macro:
- **Any function reused many times across the project** — define a macro so references update if the function moves.
- **Any syntactic sugar** — the macro must link to the **convention block** that defines the sugar (not the underlying function definition).

**Link target depends on what the macro renders as:**

| Macro type     | Renders as                                     | Links to                                 |
|----------------|------------------------------------------------|------------------------------------------|
| Function-style | `\mathrm{comm}(A, B)` — operator name + parens | `def:...` directly                       |
| Sugar          | `[A, B]` or `|a|` — alternative shorthand      | `conv:...` → which then links to the def |

Pattern:
```latex
\providecommand*{\absoluteValueRef}{project::lec01::conv:absolute-value-notation}
\newcommand*{\abs}[1]{\xref[\lvert#1\rvert]{\absoluteValueRef}}
```

Operator macros use `\mathop{...}` for correct spacing (as with `\sup`, `\seqlim`).

### 8. Juxtaposition

**Multiplication should use an explicit operator:**

- Variable × variable: write `c \cdot \varepsilon`, never `c\varepsilon`.
- Literal number × variable: `2x`, `3n`, `10^{-k}` tolerated where unambiguous. `\cdot` recommended.
- When in doubt, use `\cdot`.

**Function application preferably uses parentheses:**

- `f(x)`, `d(x, y)`, `\seqlim(a)` — preferred form.
- Bare juxtaposition for application (e.g., `fx`, or `gf` for composition) tolerated where context is unambiguous and matches related areas (type theory, abstract algebra).
- When juxtaposing without parens, **always insert a small separator**: `f\,x`, not `fx`.
