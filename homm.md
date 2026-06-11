# Практика: HoMM

## 1. Описание предметной области и сущностей
*В системе моделируется взаимодействие игрока с объектами на игровой карте. Интерфейсы IArmyHolder, ITreasureHolder и IOwnable определяют роли объектов: наличие армии, сокровищ и владельца. Player - игрок, который может победить армию, умереть и получить сокровища. Army - армия для защиты объектов. Treasure - сокровища. Dwelling - жилище. Mine - шахта, реализует все три интерфейса. Creeps - монстры, имеют армию и сокровища. Wolves - волки, имеют только армию. ResourcePile - куча ресурсов. Interaction - статический класс с методом Make, который управляет взаимодействием во всей игре. Он проверяет тип объекта через интерфейсы, вызывает TryFight для проверки боя и в зависимости от результата игрок либо умирает, либо забирает сокровища и захватывает объект.*

## 2. Диаграмма классов (Mermaid)
```mermaid
classDiagram
    direction TB

    class ITreasureHolder {
        <<interface>>
        +Treasure Treasure
    }

    class IArmyHolder {
        <<interface>>
        +Army Army
    }

    class IOwnable {
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

    IOwnable <|.. Dwelling : Реализует
    IOwnable <|.. Mine : Реализует
    IArmyHolder <|.. Mine : Реализует
    ITreasureHolder <|.. Mine : Реализует
    IArmyHolder <|.. Creeps : Реализует
    ITreasureHolder <|.. Creeps : Реализует
    IArmyHolder <|.. Wolves : Реализует
    ITreasureHolder <|.. ResourcePile : Реализует

    Interaction ..> Player : Использует
    Interaction ..> IArmyHolder : Проверяет тип
    Interaction ..> ITreasureHolder : Проверяет тип
    Interaction ..> IOwnable : Проверяет тип

    IArmyHolder o-- Army : Содержит
    ITreasureHolder o-- Treasure : Содержит
```
