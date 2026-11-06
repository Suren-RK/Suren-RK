# DBMS Unit I — Relational Model Foundations

These notes cover the basic relational-database terms used when designing tables and writing SQL queries.

## 1. What is a relational database?

A relational database stores related data in **tables (relations)**. A table contains rows and columns:

- **Relation:** a table.
- **Tuple:** one row (one record).
- **Attribute:** one column (one property).
- **Domain:** the permitted set of values for an attribute.

Example relation: `STUDENT`

| student_id | name | department |
|---:|---|---|
| 101 | Asha | AI&DS |
| 102 | Ravi | CSE |
| 103 | Meena | ECE |

For this table:

- Attributes: `student_id`, `name`, `department` (degree = 3).
- Tuples: 3 student records (cardinality = 3).
- The domain of `student_id` could be positive integers.

> Remember: degree counts columns; cardinality counts rows.

## 2. Schema vs. instance

| Term | Meaning | Example |
|---|---|---|
| **Schema** | The database's defined structure | `STUDENT(student_id, name, department)` |
| **Instance** | The actual data stored at a particular time | The three student rows above |

A schema usually changes infrequently. The instance changes whenever rows are inserted, updated, or deleted.

## 3. Keys

Keys identify rows and connect related tables.

### Super key
Any attribute or combination of attributes that uniquely identifies a row. It may contain extra attributes.

Example: if `student_id` is unique, both `{student_id}` and `{student_id, name}` are super keys.

### Candidate key
A **minimal super key**: no attribute can be removed while keeping uniqueness.

Example: `{student_id}` is a candidate key if it uniquely identifies each student.

### Primary key
The candidate key selected to identify rows. It must be unique and cannot be NULL.

Example: `student_id` is selected as the primary key of `STUDENT`.

### Foreign key
An attribute (or attribute set) in one table that refers to a candidate key—commonly the primary key—of another table.

Example:

```text
DEPARTMENT(dept_id, dept_name)
STUDENT(student_id, name, dept_id)
```

Here, `STUDENT.dept_id` references `DEPARTMENT.dept_id`. This represents which department each student belongs to.

## 4. Basic integrity constraints

- **Domain constraint:** an attribute value must belong to its allowed domain.
- **Key constraint:** key values must uniquely identify tuples.
- **Entity integrity:** a primary-key value cannot be NULL.
- **Referential integrity:** a foreign-key value must match a referenced key value, unless NULL is allowed by the schema.

These rules help prevent invalid or inconsistent data.

## 5. Small SQL example

```sql
CREATE TABLE Department (
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(40) NOT NULL
);

CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(40) NOT NULL,
    dept_id INT,
    FOREIGN KEY (dept_id) REFERENCES Department(dept_id)
);

INSERT INTO Department VALUES (1, 'AI&DS');
INSERT INTO Student VALUES (101, 'Asha', 1);

SELECT student_id, name
FROM Student
WHERE dept_id = 1;
```

The department row must exist before a student row can reference its `dept_id`.

## 6. Quick revision

1. What is a tuple? — A row in a relation.
2. What is an attribute? — A column in a relation.
3. Degree of a table with 5 columns? — 5.
4. Cardinality of a table with 12 rows? — 12.
5. What is the difference between a super key and a candidate key? — A candidate key is a minimal super key.
6. Can a primary key contain NULL? — No.
7. What does a foreign key help enforce? — Referential integrity.

## 7. Check your understanding

Given `COURSE(course_id, title, dept_id)`:

- If `course_id` uniquely identifies every course, it can be the primary key.
- If `dept_id` refers to `DEPARTMENT(dept_id)`, it is a foreign key.
- If the table has 4 columns and 20 rows, its degree is 4 and cardinality is 20.

**Study tip:** Before writing a query, identify the table, its attributes, the key columns, and how the tables are related. This makes joins and constraints easier to understand.
