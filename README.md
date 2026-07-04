# octonion_kmap_lab
# Boolean Algebra to Eight Dimensions

How the three-variable Boolean cube climbs, two different ways, into the two most
exceptional structures in eight dimensions — the **octonions** and the **E8 lattice** —
and why those two turn out to be the *same* eight dimensions.

Everything here is verifiable; the scripts in this repo check each claim numerically.

---

## 0. The seed: the 3-bit Boolean cube $\mathrm{GF}(2)^3$

Take the eight bit-strings $000, 001, \dots, 111$ — the eight cells of a three-variable
Karnaugh map. Under bitwise XOR they form a vector space over the two-element field:

$$
\mathrm{GF}(2)^3 \;=\; \{0,1\}^3, \qquad a \oplus b = \text{bitwise XOR}.
$$

Two facts about this cube do all the work below:

- **Adjacency is a single bit-flip.** Neighbouring K-map cells differ in one coordinate; this is the Hamming-cube graph $Q_3$.
- **The seven nonzero cells fall into seven triples** $\{a, b, a\oplus b\}$. These are the seven lines of the **Fano plane** $PG(2,2)$ — the smallest projective plane. Each line is closed under XOR: $a \oplus b \oplus (a\oplus b) = 0$.

That's the entire combinatorial input. Both liftings are this cube plus one extra ingredient — a **sign**.

---

## 1. Lift one — the cube becomes the octonions $\mathbb{O}$

### 1.1 Start flat

Form the group algebra $\mathbb{R}[\mathrm{GF}(2)^3]$: basis $\{e_a : a \in \mathrm{GF}(2)^3\}$, multiplication

$$
e_a \, e_b = e_{a \oplus b}.
$$

This is a perfectly good 8-dimensional algebra, but it is commutative and associative —
geometrically inert. Nothing rotates.

### 1.2 Add the twist

Now twist the product by a sign $\varepsilon(a,b) \in \{\pm 1\}$:

$$
\boxed{\,e_a \, e_b \;=\; \varepsilon(a,b)\, e_{a \oplus b}\,}
$$

with

- $\varepsilon(0,\cdot) = \varepsilon(\cdot,0) = 1$ &nbsp; ($e_0 = 1$ is the identity),
- $e_a^2 = -1$ for $a \neq 0$ &nbsp; (the seven nonzero units are **imaginary**),
- $\varepsilon(a,b) = -\varepsilon(b,a)$ for distinct nonzero $a,b$ &nbsp; (imaginary units **anticommute**).

The index arithmetic is still pure XOR — pure Boolean algebra. The only new data is $\varepsilon$,
one bit per ordered pair. Concretely, $\varepsilon$ is fixed by giving each of the seven Fano
lines a cyclic orientation (`octonion_kmap.py` searches the $2^7$ orientations and keeps one that works).

### 1.3 Why the twist gives $\mathbb{O}$ and not junk

For the right $\varepsilon$, the algebra is **non-associative** but still **alternative** and,
crucially, **norm-multiplicative**:

$$
|xy| = |x|\,|y| \quad \text{for all } x,y.
$$

By **Hurwitz's theorem**, the only real algebras with a multiplicative norm are
$\mathbb{R}, \mathbb{C}, \mathbb{H}, \mathbb{O}$ — dimensions $1, 2, 4, 8$. Ours is 8-dimensional,
so it *is* the octonions. (`octonion_kmap.py` verifies $|xy|=|x||y|$ over random inputs.)

In cohomological language: $\varepsilon$ is a 2-cochain on $\mathrm{GF}(2)^3$ whose coboundary is a
non-trivial 3-cocycle; that 3-cocycle *is* the failure of associativity. Octonions as a twisted
group algebra of $(\mathbb{Z}/2)^3$ is a theorem of **Albuquerque & Majid (1999)**.

### 1.4 The payoff: Boolean cells rotate $\mathbb{R}^8$

For each imaginary unit $e_a$, left-multiplication $L_{e_a}: x \mapsto e_a x$ is a linear map of
$\mathbb{R}^8$. Because the norm is multiplicative, $L_{e_a}$ is **orthogonal**, and because
$e_a^2 = -1$ (with alternativity),

$$
L_{e_a}^2 = -I.
$$

So $L_{e_a}$ is a $90^\circ$ rotation — it splits $\mathbb{R}^8$ into four planes and spins each.
**Which K-map cell you multiply by chooses the rotation.** That is Boolean algebra rotating in
eight dimensions, and it is exactly what the interactive `octonion_kmap_lab.html` displays.

---

## 2. Lift two — a Boolean *code* becomes the E8 lattice

The second climb starts from the same cube but keeps *linear subspaces* of it — error-correcting codes.

### 2.1 Reed–Muller = bounded-degree Boolean functions

The Reed–Muller code $\mathrm{RM}(r,m)$ is the set of Boolean functions on $m$ variables of
algebraic-normal-form (Zhegalkin) degree $\le r$, listed as their $2^m$-bit truth tables. In particular

$$
\mathrm{RM}(1,3) \;=\; \{\text{degree-}\le 1 \text{ functions on } 3 \text{ variables}\}
\;=\; [8,4,4] \text{ extended Hamming code},
$$

a self-dual, doubly-even (**Type II**) binary code. (`zhegalkin.py` and the checks confirm
$\mathrm{RM}(1,3)$ and the extended Hamming code are the *same* code, not merely equivalent.)

### 2.2 Construction A lifts a binary code to a lattice

Given a binary code $C \subseteq \mathrm{GF}(2)^n$,

$$
\Lambda_A(C) \;=\; \{\, x \in \mathbb{Z}^n : x \bmod 2 \in C \,\}.
$$

Applied to the $[8,4,4]$ code and rescaled,

$$
\boxed{\,E_8 \;=\; \tfrac{1}{\sqrt 2}\,\Lambda_A\big(\mathrm{RM}(1,3)\big)\,}
$$

— the unique even unimodular lattice in dimension 8, the densest sphere packing in $\mathbb{R}^8$
(**Viazovska, 2017**). Its 240 minimal vectors are the E8 roots. So an 8-bit error-correcting code
is the combinatorial seed of the 8-dimensional lattice. (`e8_graph.py` builds the 240 roots and
projects them onto the Coxeter plane — the 8 rings of 30, $h=30$.)

---

## 3. The loop closes: E8 *is* the integral octonions

The two climbs land in the same place. The maximal order of the octonions — the **integral
octonions** (Coxeter, 1946) — is, under the norm form, isometric to the E8 lattice; its **240 unit
integral octonions correspond exactly to the 240 roots** of E8. Lift one produces the *algebra*;
lift two produces the *lattice*; they are the same eight dimensions viewed two ways. The Hadamard
matrix even reappears here: the block-diagonal $\tfrac12 H_4 \oplus \tfrac12 H_4$, together with the
$D_8$ Weyl group, generates the full symmetry group $W(E_8)$ (verified: 240/240 roots preserved).

---

## 4. Why 8 — and why it stops there

The lift is real, but it is an **exceptional coincidence**, not a general ladder:

- **Hurwitz** caps normed division algebras at dimensions $1,2,4,8$. Eight is the ceiling; there is no 16-dimensional division algebra (the sedenions lose the norm property).
- **E8 from Reed–Muller is frozen at $m=3$**, the unique place where first-order RM, the extended Hamming code, and self-duality coincide ($1 = m-2 \Rightarrow m = 3$). For $m>3$, Reed–Muller feeds the **Barnes–Wall** lattices (via the multilevel Construction D), not E8.

Both facts single out $m=3$, dimension $8$. That is why the story lives here and nowhere else.

---

## 5. What this is, and what it is not

The honest boundary matters:

- **The bones are Boolean.** Index arithmetic is XOR; codes are $\mathrm{GF}(2)$-subspaces. The combinatorics is genuinely just 0s and 1s.
- **The geometry is in the twist.** Strip the sign cocycle and octonion multiplication collapses to the inert commutative algebra of §1.1; strip the mod-2 gluing and Construction A collapses to $\mathbb{Z}^8$. The rotations and the dense packing live entirely in that one extra layer of sign/parity data.
- **Scope.** This document derives and verifies the *lift itself*. It makes no claim that the lift explains anything downstream (machine-learning pruning, physics, etc.); those are separate hypotheses and are out of scope here.

---

## Reproduce

| Script | Checks |
|---|---|
| `octonion_kmap.py` | Builds $\mathbb{O}$ from the K-map + a sign cocycle; verifies $\lvert xy\rvert=\lvert x\rvert\lvert y\rvert$, $L_{e_a}^2=-I$, non-associativity |
| `octonion_kmap_lab.html` | Interactive: multiply cells, watch each unit rotate $\mathbb{R}^8$, live composition check |
| `zhegalkin.py` | ANF / Möbius transform; monomial support = multi-controlled-NOT gate count |
| `e8_graph.py` | 240 E8 roots; Coxeter-plane projection (8 rings of 30, $h=30$) |

## References

- M. A. Albuquerque, S. Majid, *Quasialgebra structure of the octonions*, J. Algebra 220 (1999).
- J. Baez, *The Octonions*, Bull. AMS 39 (2002).
- H. S. M. Coxeter, *Integral Cayley numbers*, Duke Math. J. 13 (1946).
- J. H. Conway, N. J. A. Sloane, *Sphere Packings, Lattices and Groups*.
- M. Viazovska, *The sphere packing problem in dimension 8*, Ann. of Math. 185 (2017).
- Bogdanov et al., *Representation of Boolean functions in terms of quantum computation*, arXiv:1906.06374.
