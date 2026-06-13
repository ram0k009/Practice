# Практика: Роботы

## 1. Описание предметной области и сущностей
*В системе моделируются роботы с искусственным интеллектом, выполняющие команды. IRobotAI<TCommand> - интерфейс ИИ робота, определяющий получение команды. RobotAI<TCommand> - класс для всех ИИ роботов. ShooterAI - ИИ стрелка, который с каждым шагом увеличивает step и возвращает команду движения с стрельбой. BuilderAI - ИИ строитель, который с каждым шагом увеличивает step и возвращает строительную команду. IDevice<TCommand> - интерфейс устройства, исполняющего команды. Device<TCommand> - класс для устройств. Mover - устройство, исполняющее команды движения. ShooterMover - устройство стрелка, исполняющее команды движения с стрельбой. Robot<TCommand> - робот, объединяющий ИИ и устройство, запускающий выполнение команд на определенное количество шагов и возвращающий результаты. RobotStatic - статический класс для создания роботов любого типа.*

## 2. Диаграмма классов (Mermaid)
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
