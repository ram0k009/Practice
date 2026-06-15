classDiagram
    direction TB

    class Algebra {
        -Dictionary~string, Func~Expression, Expression~~ d$
        +Differentiate(Expression~Func~double, double~~ f)$ Expression~Func~double, double~~
        -Derive(Expression node, ParameterExpression x)$ Expression
        -DeriveBinary(BinaryExpression b, ParameterExpression x)$ Expression
        -DeriveMethodCall(MethodCallExpression m, ParameterExpression x)$ Expression
    }

    class Dictionary~TKey, TValue~ {
        +Add(TKey key, TValue value) void
        +ContainsKey(TKey key) bool
    }

    class Expression {
        +Constant(object value)$ ConstantExpression
        +Add(Expression left, Expression right)$ BinaryExpression
        +Multiply(Expression left, Expression right)$ BinaryExpression
        +Call(MethodInfo method, Expression argument)$ MethodCallExpression
        +Negate(Expression expression)$ UnaryExpression
        +Lambda~TDelegate~(Expression body, ParameterExpression[] parameters)$ Expression~TDelegate~
    }

    class ParameterExpression {
        +string Name
    }

    class BinaryExpression {
        +Expression Left
        +Expression Right
        +ExpressionType NodeType
    }

    class MethodCallExpression {
        +MethodInfo Method
        +IReadOnlyList~Expression~ Arguments
    }

    Expression <|-- ParameterExpression : Наследует
    Expression <|-- BinaryExpression : Наследует
    Expression <|-- MethodCallExpression : Наследует

    Algebra --> Dictionary~string, Func~Expression, Expression~~ : Содержит статическое поле d
    Algebra ..> Expression : Использует для построения дерева выражений
    Algebra ..> ParameterExpression : Использует как параметр дифференцирования
    Algebra ..> BinaryExpression : Использует в DeriveBinary
    Algebra ..> MethodCallExpression : Использует в DeriveMethodCall