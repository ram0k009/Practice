classDiagram
    direction TB

    class Extensions {
        <<static>>
        +GetDigitsFromEnd(long number)$ IEnumerable~int~
        +WeightedSum(IEnumerable~int~ digits, int firstWeight, int secondWeight)$ int
        +ComplementToModulo(int sum, int modulo)$ int
    }

    class ControlDigitAlgo {
        <<static>>
        +Upc(long number)$ int
        +Isbn10(long number)$ int
        +Luhn(long number)$ int
        -CalcIsbn10WeightedSum(long number)$ int
        -CalcLuhnSum(IEnumerable~int~ digits)$ int
    }

    class IEnumerable~T~ {
        <<System>>
        <<interface>>
    }

    ControlDigitAlgo ..> Extensions : Вызывает методы расширения
    ControlDigitAlgo ..> IEnumerable~T~ : Использует в CalcLuhnSum
    Extensions ..> IEnumerable~T~ : Возвращает и принимает как параметр