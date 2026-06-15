# Практика: Дифференцирование

## 1. Описание предметной области и сущностей
*В системе реализовано символьное дифференцирование математических выражений. Algebra - класс, предоставляющий метод Differentiate для преобразования выражения функции в ее производную. Expression - абстрактный класс, представляющий узел дерева выражения. ParameterExpression - параметр функции. ConstantExpression - константное значение. BinaryExpression - бинарная операция. MethodCallExpression - вызов математической функции. UnaryExpression - унарная операция. Func<double, double> - делегат функции одной переменной. Dictionary<string, Func<Expression, Expression>> - словарь, сопоставляющий имя функции делегату, строящему выражение ее производной.*

## 2. Диаграмма классов (Mermaid)
```mermaid
classDiagram
    direction TB

    class Algebra {
        -Dictionary d$
        +Differentiate(Expression f)$ Expression
        -Derive(Expression node, ParameterExpression x)$ Expression
        -DeriveBinary(BinaryExpression b, ParameterExpression x)$ Expression
        -DeriveMethodCall(MethodCallExpression m, ParameterExpression x)$ Expression
    }

    class Dictionary~string, Func~Expression, Expression~~ {
        <<System>>
    }

    class Func~Expression, Expression~ {
        <<System>>
        <<delegate>>
    }

    class Expression {
        <<System>>
    }

    class ParameterExpression {
        <<System>>
    }

    class BinaryExpression {
        <<System>>
    }

    class MethodCallExpression {
        <<System>>
    }

    Algebra --> Dictionary~string, Func~Expression, Expression~~ : d (статическое поле)
    Dictionary~string, Func~Expression, Expression~~ --> Func~Expression, Expression~ : Хранит делегаты
    Algebra ..> Expression : Использует как тип возврата и параметров
    Algebra ..> ParameterExpression : Использует как параметр метода
    Algebra ..> BinaryExpression : Использует в DeriveBinary
    Algebra ..> MethodCallExpression : Использует в DeriveMethodCall
```
