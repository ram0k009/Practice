```mermaid
classDiagram
    direction TB

    class IRobotAI~TCommand~ {
        <<interface>>
        +TCommand GetCommand()
    }

    class RobotAI~TCommand~ {
        <<abstract>>
        +TCommand GetCommand()*
    }

    class ShooterAI {
        -int step
        +IShooterMoveCommand GetCommand()
    }

    class BuilderAI {
        -int step
        +BuilderCommand GetCommand()
    }

    class IDevice~TCommand~ {
        <<interface>>
        +string ExecuteCommand(TCommand command)
    }

    class Device~TCommand~ {
        <<abstract>>
        +string ExecuteCommand(TCommand command)*
    }

    class Mover {
        +string ExecuteCommand(IMoveCommand command)
    }

    class ShooterMover {
        +string ExecuteCommand(IShooterMoveCommand command)
    }

    class Robot~TCommand~ {
        -Func~string~ getNext
        -Robot(Func~string~ getNext)
        +IEnumerable~string~ Start(int steps)
        +Robot~TCommand~ Create(IRobotAI~TCommand~ ai, IDevice~TCommand~ device)$
    }

    class RobotStatic {
        <<static>>
        +Robot~TCommand~ Create~TCommand~(IRobotAI~TCommand~ ai, IDevice~TCommand~ device)$
    }

    IRobotAI~TCommand~ <|.. RobotAI~TCommand~ : Реализует
    RobotAI~IShooterMoveCommand~ <|-- ShooterAI : Наследует
    RobotAI~BuilderCommand~ <|-- BuilderAI : Наследует
    IDevice~TCommand~ <|.. Device~TCommand~ : Реализует
    Device~IMoveCommand~ <|-- Mover : Наследует
    Device~IShooterMoveCommand~ <|-- ShooterMover : Наследует
    Robot~TCommand~ --> IRobotAI~TCommand~ : Использует интерфейс
    Robot~TCommand~ --> IDevice~TCommand~ : Использует интерфейс
    RobotStatic ..> Robot~TCommand~ : Создает экземпляр
```
