# DBMS — Lossless and Lossy Decomposition

**Topic:** Relational database design · Decomposition  
**Purpose:** Exam revision notes with examples and the binary lossless-join test.

## 1. What is decomposition?

Decomposition means splitting one relation (table) into two or more smaller relations to reduce redundancy and avoid insertion, update, and deletion anomalies.

A good decomposition should ideally:
- be **lossless**, so joining the smaller relations recreates exactly the original relation;
- preserve dependencies, so important functional dependencies can still be checked without repeatedly joining tables.

These are separate properties: a decomposition can be lossless without being dependency-preserving.

## 2. Lossless-join decomposition

A decomposition is **lossless** when the natural join of the decomposed relations gives exactly the original relation—no information is lost and no extra (spurious) tuples are created.

### Example

Suppose the original relation is:

**R(A, B, C)**

| A | B | C |
|---|---|---|
| 1 | X | 10 |
| 2 | Y | 20 |

Assume the functional dependency **A → B** holds.

Decompose it into:
- **R1(A, B)**
- **R2(A, C)**

The common attribute is **A**. Since **A → B**, the common attribute determines all attributes of R1. Joining R1 and R2 on A recreates exactly the two original rows.

For a binary decomposition of R into R1 and R2, the lossless-join test under functional dependencies is:

`(R1 ∩ R2) → R1` **or** `(R1 ∩ R2) → R2`

The dependency must be implied by the given functional dependencies (that is, it must belong to their closure).

## 3. Lossy-join decomposition

A decomposition is **lossy** when joining the smaller relations produces extra rows called **spurious tuples**. The original relation cannot be recovered exactly.

### Example

Original relation **R(A, B, C)**:

| A | B | C |
|---|---|---|
| 1 | X | 10 |
| 2 | X | 20 |

Decompose into:
- **R1(A, B)** = {(1, X), (2, X)}
- **R2(B, C)** = {(X, 10), (X, 20)}

The common attribute is B. Here, B does not determine A or C.

Joining on B = X produces:

| A | B | C | Result |
|---|---|---|---|
| 1 | X | 10 | Original |
| 1 | X | 20 | Spurious |
| 2 | X | 10 | Spurious |
| 2 | X | 20 | Original |

The join creates two extra combinations. Therefore, this decomposition is **lossy**.

## 4. Lossless vs. lossy — quick comparison

| Lossless decomposition | Lossy decomposition |
|---|---|
| Reconstructs the original relation exactly | Does not reconstruct it exactly |
| Does not create spurious tuples | Creates spurious tuples |
| Preserves all original information | Can introduce incorrect combinations |
| Desired in good database design | Must be avoided |

## 5. How to answer a binary decomposition question

1. Write the two decomposed relations, R1 and R2.
2. Find their common attributes: `R1 ∩ R2`.
3. Use the given functional dependencies to check whether the common attributes determine all attributes of R1 or all attributes of R2.
4. If either condition holds, the decomposition is lossless.
5. If neither condition holds, it is lossy under the binary FD test.

**Important:** Do not decide that a decomposition is lossless just because the smaller tables contain all the original columns. The join condition matters.

## 6. Short exam answers

**Q: Define lossless decomposition.**  
A: A decomposition is lossless if joining the decomposed relations gives exactly the original relation without losing information or generating spurious tuples.

**Q: Define lossy decomposition.**  
A: A decomposition is lossy if joining the decomposed relations generates spurious tuples, so the original relation is not recovered exactly.

**Q: What is a spurious tuple?**  
A: A spurious tuple is an extra, incorrect row produced when decomposed relations are joined.

**Q: State the lossless-join condition for a binary decomposition.**  
A: A decomposition of R into R1 and R2 is lossless if the common attributes functionally determine all attributes of R1 or all attributes of R2, under the given functional dependencies.

## 7. Check your understanding

1. Why is lossless decomposition important?
2. What is the difference between a spurious tuple and an original tuple?
3. If R1 ∩ R2 = {A} and A → R1 holds, is the binary decomposition lossless?
4. Does lossless decomposition automatically guarantee dependency preservation?

**Answers:** 1. It allows exact reconstruction of the original data. 2. A spurious tuple is an extra row created by the join. 3. Yes. 4. No; they are separate properties.
