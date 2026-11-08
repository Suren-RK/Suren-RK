# Armstrong's Axioms, Attribute Closure and Candidate Keys

**DBMS Unit II · Functional dependencies**

## 1. Functional dependency (FD)

A functional dependency (X \to Y) means that, in every valid instance of a relation, each value of attribute set (X) determines exactly one value of attribute set (Y).

- (X): determinant (left-hand side)
- (Y): dependent attributes (right-hand side)

Example: `StudentID → StudentName` means one StudentID identifies one StudentName.

An FD is a rule about the meaning of the data, not merely a pattern noticed in a small sample.

## 2. Armstrong's three basic axioms

Armstrong's axioms are sound and complete inference rules for functional dependencies.

### Axiom 1 — Reflexivity

If (Y \subseteq X), then (X \to Y).

Example: ({StudentID, CourseID} \to StudentID).

### Axiom 2 — Augmentation

If (X \to Y), then (XZ \to YZ), for any attribute set (Z).

Example: If (StudentID \to StudentName), then:

({StudentID, CourseID} \to {StudentName, CourseID}).

### Axiom 3 — Transitivity

If (X \to Y) and (Y \to Z), then (X \to Z).

Example:
- (StudentID \to DepartmentID)
- (DepartmentID \to DepartmentName)
- Therefore, (StudentID \to DepartmentName).

## 3. Useful rules derived from the axioms

| Rule | Inference |
|---|---|
| Union (additivity) | If (X \to Y) and (X \to Z), then (X \to YZ). |
| Decomposition (projectivity) | If (X \to YZ), then (X \to Y) and (X \to Z). |
| Pseudotransitivity | If (X \to Y) and (WY \to Z), then (WX \to Z). |

These are derived rules; they do not replace the three basic axioms.

## 4. Attribute closure

The **closure of an attribute set (X)**, written (X^+), is the set of all attributes that can be functionally determined by (X) using a given set of FDs.

### Algorithm to find (X^+)

1. Start with (X^+ = X).
2. Examine every FD (L \to R).
3. If all attributes in (L) are already in (X^+), add the attributes in (R).
4. Repeat until no new attributes can be added.

### Worked example

Consider relation:

`ENROLLMENT(StudentID, CourseID, StudentName, CourseName, Grade)`

Given FDs:

1. `StudentID → StudentName`
2. `CourseID → CourseName`
3. `StudentID, CourseID → Grade`

Find ({StudentID, CourseID}^+).

| Step | Rule applied | Closure |
|---:|---|---|
| 1 | Start | {StudentID, CourseID} |
| 2 | StudentID → StudentName | {StudentID, CourseID, StudentName} |
| 3 | CourseID → CourseName | {StudentID, CourseID, StudentName, CourseName} |
| 4 | StudentID, CourseID → Grade | {StudentID, CourseID, StudentName, CourseName, Grade} |

No more attributes can be added.

Therefore:

[
{StudentID, CourseID}^+ = \{StudentID, CourseID, StudentName, CourseName, Grade\}
]

## 5. Candidate keys using closure

A **superkey** is an attribute set whose closure contains every attribute of the relation.

A **candidate key** is a minimal superkey: no proper subset of it is also a superkey.

For the ENROLLMENT relation:

- ({StudentID, CourseID}^+) contains every attribute, so the pair is a superkey.
- (StudentID^+ = {StudentID, StudentName}), so StudentID alone is not a key.
- (CourseID^+ = {CourseID, CourseName}), so CourseID alone is not a key.
- Neither attribute can be removed from the pair.

Hence, the candidate key is **{StudentID, CourseID}**.

### Helpful shortcut

Any attribute that does not appear on the right-hand side of any FD must be included in every candidate key (under the given FD set). It cannot be derived from other attributes.

Use this as a starting clue, then calculate closures and check minimality. It is not a substitute for the full key test.

## 6. Testing whether an FD is implied

To test whether (X \to Y) follows from a set of FDs (F):

1. Calculate (X^+) using (F).
2. Check whether every attribute in (Y) belongs to (X^+).
3. If yes, (F) implies (X \to Y); otherwise, it does not.

## 7. Common exam mistakes

- A superkey need not be minimal; a candidate key must be minimal.
- Do not stop closure calculation after one pass if a newly added attribute can trigger another FD.
- Apply an FD only when its entire left-hand side is present in the closure.
- Attribute closure depends on the supplied FD set.
- A sample table cannot prove that an FD always holds; the business rules determine the FDs.

## 8. Quick practice

Use the ENROLLMENT FDs above.

1. Find (StudentID^+).
2. Find (CourseID^+).
3. Is ({StudentID, CourseID}) a superkey?
4. Is ({StudentID, CourseID, Grade}) a candidate key?
5. Does the FD (StudentID \to CourseName) follow from the given FDs?

**Answers:**  
1. ({StudentID, StudentName})  
2. ({CourseID, CourseName})  
3. Yes.  
4. No; it is a non-minimal superkey because the pair already determines every attribute.  
5. No; (StudentID^+) does not contain CourseName.

## Key takeaway

Armstrong's axioms derive valid FDs. Attribute closure finds everything an attribute set determines. Candidate keys are the minimal attribute sets whose closure contains the entire relation.
