classDiagram
    direction TB

    class IHasArmy {
        <<interface>>
        +Army Army
    }

    class IHasTreasure {
        <<interface>>
        +Treasure Treasure
    }

    class ICanBeOwned {
        <<interface>>
        +int Owner
    }

    class Dwelling {
        +int Owner
    }

    class Mine {
        +int Owner
        +Army Army
        +Treasure Treasure
    }

    class Creeps {
        +Army Army
        +Treasure Treasure
    }

    class Wolves {
        +Army Army
    }

    class ResourcePile {
        +Treasure Treasure
    }

    class Interaction {
        <<static>>
        +static void Make(Player player, object mapObject)
        -static bool TryFight(Player player, object mapObject)
    }

    class Player {
        +bool CanBeat(Army army)
        +void Die()
        +void Consume(Treasure treasure)
        +int Id
    }

    class Army {
    }

    class Treasure {
    }

    ICanBeOwned <|.. Dwelling : Реализует
    ICanBeOwned <|.. Mine : Реализует
    IHasArmy <|.. Mine : Реализует
    IHasTreasure <|.. Mine : Реализует
    IHasArmy <|.. Creeps : Реализует
    IHasTreasure <|.. Creeps : Реализует
    IHasArmy <|.. Wolves : Реализует
    IHasTreasure <|.. ResourcePile : Реализует

    Interaction ..> Player : Использует
    Interaction ..> IHasArmy : Проверяет тип
    Interaction ..> IHasTreasure : Проверяет тип
    Interaction ..> ICanBeOwned : Проверяет тип

    IHasArmy o-- Army : Содержит
    IHasTreasure o-- Treasure : Содержит
