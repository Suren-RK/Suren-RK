# Infix to Postfix Conversion Using a Stack

**Topic:** DSA · Stacks · Expression processing

## The key idea

In postfix notation, operators come **after** their operands.

- Infix: `A + B`
- Postfix: `AB+`

During conversion:
- **Operands** (letters or digits) go directly to the output.
- **Operators** wait in a stack until precedence and associativity tell us when to output them.
- Parentheses control which operators are popped first.

The operator stack stores pending operators; it does not evaluate the expression.

## Operator rules

| Operator | Precedence | Associativity |
|---|---:|---|
| `^` | 3 (highest) | Right to left |
| `*`, `/` | 2 | Left to right |
| `+`, `-` | 1 (lowest) | Left to right |

When a new operator arrives:
1. Pop an operator from the stack if it has **higher precedence**.
2. Also pop if precedence is equal and the incoming operator is left-associative.
3. Stop when the stack is empty, the top is `(`, or the top should stay because it has lower precedence.
4. Push the incoming operator.

For `^`, equal precedence does not cause a pop because exponentiation is right-associative.

## Walkthrough: `A + B * C`

| Scanned token | Action | Output | Operator stack (bottom → top) |
|---|---|---|---|
| `A` | Send operand to output | `A` | empty |
| `+` | Push operator | `A` | `+` |
| `B` | Send operand to output | `AB` | `+` |
| `*` | Keep `+` below it because `*` has higher precedence; push `*` | `AB` | `+, *` |
| `C` | Send operand to output | `ABC` | `+, *` |
| End | Pop remaining operators: `*`, then `+` | `ABC*+` | empty |

**Result:** `ABC*+`

Why are two operators allowed in the stack? Because `*` must be applied before `+`. If we popped `+` as soon as `*` arrived, the result would incorrectly become `AB+C*`.

## Parentheses example: `(A + B) * C`

1. Push `(`.
2. Output `A`; push `+`.
3. Output `B`.
4. At `)`, pop `+` to output, then discard the matching `(`.
5. Push `*`; output `C`.
6. At the end, pop `*`.

**Result:** `AB+C*`

A closing parenthesis causes operators to be popped up to the matching opening parenthesis. The parentheses themselves are not included in postfix output.

## Java implementation

This version assumes valid expressions with single-character operands and no spaces (spaces are skipped). It supports `+`, `-`, `*`, `/`, `^`, and parentheses.

```java
import java.util.ArrayDeque;
import java.util.Deque;

class InfixToPostfix {
    static int precedence(char op) {
        switch (op) {
            case '^': return 3;
            case '*':
            case '/': return 2;
            case '+':
            case '-': return 1;
            default: return 0;
        }
    }

    static boolean isOperator(char ch) {
        return ch == '+' || ch == '-' || ch == '*'
            || ch == '/' || ch == '^';
    }

    static String convert(String infix) {
        StringBuilder postfix = new StringBuilder();
        Deque<Character> stack = new ArrayDeque<>();

        for (int i = 0; i < infix.length(); i++) {
            char ch = infix.charAt(i);

            if (Character.isWhitespace(ch)) {
                continue;
            }

            if (Character.isLetterOrDigit(ch)) {
                postfix.append(ch);
            } else if (ch == '(') {
                stack.push(ch);
            } else if (ch == ')') {
                while (!stack.isEmpty() && stack.peek() != '(') {
                    postfix.append(stack.pop());
                }
                if (!stack.isEmpty()) {
                    stack.pop(); // discard '('
                }
            } else if (isOperator(ch)) {
                while (!stack.isEmpty() && isOperator(stack.peek())) {
                    char top = stack.peek();
                    boolean higher = precedence(top) > precedence(ch);
                    boolean equalAndLeftAssociative =
                        precedence(top) == precedence(ch) && ch != '^';

                    if (!higher && !equalAndLeftAssociative) {
                        break;
                    }
                    postfix.append(stack.pop());
                }
                stack.push(ch);
            }
        }

        while (!stack.isEmpty()) {
            postfix.append(stack.pop());
        }

        return postfix.toString();
    }

    public static void main(String[] args) {
        System.out.println(convert("A+B*C"));   // ABC*+
        System.out.println(convert("(A+B)*C")); // AB+C*
    }
}
```

## Complexity

- **Time:** O(n) — each token is scanned once and each operator is pushed and popped at most once.
- **Space:** O(n) — in the worst case, the stack can hold O(n) operators.

## Quick checks

| Infix | Postfix |
|---|---|
| `A+B` | `AB+` |
| `A+B*C` | `ABC*+` |
| `(A+B)*C` | `AB+C*` |
| `A-B-C` | `AB-C-` |
| `A^B^C` | `ABC^^` |

**Remember:** operands go straight to output; operators wait in the stack. A higher-precedence operator can sit above a lower-precedence operator until it is time to pop the stack.
