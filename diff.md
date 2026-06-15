# Практика: Дифференцирование

## 1. Описание предметной области и сущностей
*В системе реализовано символьное дифференцирование математических выражений. Algebra - класс, предоставляющий метод Differentiate для преобразования выражения функции в ее производную. Expression - абстрактный класс, представляющий узел дерева выражения. ParameterExpression - параметр функции. ConstantExpression - константное значение. BinaryExpression - бинарная операция. MethodCallExpression - вызов математической функции. UnaryExpression - унарная операция. Func<double, double> - делегат функции одной переменной. Dictionary<string, Func<Expression, Expression>> - словарь, сопоставляющий имя функции делегату, строящему выражение ее производной.*

## 2. Диаграмма классов (Mermaid)
```mermaid
classDiagram
    direction TB

    class Algebra {
        -Dictionary d$
        +Differentiate(f)$
        -Derive(node, x)$
        -DeriveBinary(b, x)$
        -DeriveMethodCall(m, x)$
    }

    class Dictionary {
        +Add() void
        +ContainsKey() bool
    }

    class Expression {
        <<abstract>>
    }

    class ParameterExpression {
        +string Name
    }

    class BinaryExpression {
        +Expression Left
        +Expression Right
    }

    class MethodCallExpression {
        +MethodInfo Method
    }

    class UnaryExpression {
        +ExpressionType NodeType
    }

    Expression <|-- ParameterExpression : Наследует
    Expression <|-- BinaryExpression : Наследует
    Expression <|-- MethodCallExpression : Наследует
    Algebra --> Dictionary : Содержит статическое поле d
    Algebra ..> Expression : Использует для построения дерева выражений
    Algebra ..> ParameterExpression : Использует как параметр дифференцирования
    Algebra ..> BinaryExpression : Использует в DeriveBinary
    Algebra ..> MethodCallExpression : Использует в DeriveMethodCall
```
