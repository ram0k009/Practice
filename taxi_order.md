# Практика: TaxiOrder

## 1. Описание предметной области и сущностей
*В системе реализовано управление заказами такси. TaxiApi - основной класс, предоставляющий API для создания и управления заказами такси. TaxiOrder - агрегатный корень, представляющий заказ такси. Driver - сущность, представляющая водителя такси с закрепленным автомобилем. Car - объект, описывающий автомобиль. PersonName - объект, хранящее имя и фамилию клиента или водителя. Address - объект, описывающий адрес. DriversRepository - репозиторий для поиска водителей по идентификатору. TaxiOrderStatus - перечисление, определяющее состояния заказа: WaitingForDriver (ожидание водителя), WaitingCarArrival (ожидание прибытия авто), InProgress (в пути), Finished (завершен), Canceled (отменен). ITaxiApi<TOrder> - интерфейс, определяющий контракт для работы с заказами такси. Entity<TId> и ValueType<T> - классы инфраструктуры для сущностей и объектов-значений.*

## 2. Диаграмма классов (Mermaid)
```mermaid
classDiagram
    direction TB

    class TaxiApi {
        -DriversRepository driverRepo
        -Func~DateTime~ currentTime
        -int idCounter
        +CreateOrderWithoutDestination(firstName, lastName, street, building) TaxiOrder
        +UpdateDestination(order, street, building) void
        +AssignDriver(order, driverId) void
        +UnassignDriver(order) void
        +Cancel(order) void
        +StartRide(order) void
        +FinishRide(order) void
        +GetDriverFullInfo(order) string
        +GetShortOrderInfo(order) string
    }

    class TaxiOrder {
        -TaxiOrderStatus currentStatus
        -DateTime createdAt
        -DateTime driverAssignedAt
        -DateTime cancelledAt
        -DateTime rideStartedAt
        -DateTime rideFinishedAt
        -Driver assignedDriver
        +PersonName ClientName
        +Address Start
        +Address Destination
        +TaxiOrderStatus Status
        +DateTime CreationTime
        +DateTime DriverAssignmentTime
        +DateTime CancelTime
        +DateTime StartRideTime
        +DateTime FinishRideTime
        +Driver Driver
        +TaxiOrder(id, clientName, start, creationTime)
        +SetDestination(destination) void
        +UpdateDestination(destination) void
        +AssignDriver(driver, assignmentTime) void
        +UnassignDriver() void
        +Cancel(cancelTime) void
        +StartRide(startTime) void
        +FinishRide(finishTime) void
        +GetDriverInfo() string
        +GetShortInfo() string
        -GetLastProgressTime() DateTime
        -FormatName(name) string
        -FormatAddress(address) string
    }

    class Driver {
        +PersonName Name
        +Car Car
        +Driver(driverId, name, car)
    }

    class Car {
        +string Color
        +string Model
        +string PlateNumber
        +Car(color, model, plateNumber)
    }

    class DriversRepository {
        +FindById(driverId) Driver
    }

    class PersonName {
        +string FirstName
        +string LastName
        +PersonName(firstName, lastName)
    }

    class Address {
        +string Street
        +string Building
        +Address(street, building)
    }

    class ValueType~Car~ {
        <<Infrastructure>>
    }

    class Entity~int~ {
        <<Infrastructure>>
    }

    class ITaxiApi~TOrder~ {
        <<interface>>
        +CreateOrderWithoutDestination(firstName, lastName, street, building) TOrder
        +UpdateDestination(order, street, building) void
        +AssignDriver(order, driverId) void
        +UnassignDriver(order) void
        +Cancel(order) void
        +StartRide(order) void
        +FinishRide(order) void
        +GetDriverFullInfo(order) string
        +GetShortOrderInfo(order) string
    }

    class TaxiOrderStatus {
        <<enumeration>>
        WaitingForDriver
        WaitingCarArrival
        InProgress
        Finished
        Canceled
    }

    class Func~DateTime~ {
        <<System>>
        <<delegate>>
    }

    Entity~int~ <|-- TaxiOrder : Наследует
    Entity~int~ <|-- Driver : Наследует
    ValueType~Car~ <|-- Car : Наследует
    ITaxiApi~TaxiOrder~ <|.. TaxiApi : Реализует
    TaxiApi --> DriversRepository : driverRepo
    TaxiApi --> Func~DateTime~ : currentTime
    TaxiOrder *-- PersonName : ClientName
    TaxiOrder *-- Address : Start
    TaxiOrder *-- Address : Destination
    Driver *-- PersonName : Name
    Driver *-- Car : Car
    TaxiOrder o-- Driver : assignedDriver
    TaxiOrder --> TaxiOrderStatus : Status
    TaxiApi ..> TaxiOrder : Создает
    TaxiApi ..> Address : Создает для передачи
    TaxiApi ..> PersonName : Создает для передачи
    TaxiApi ..> Driver : Использует при назначении
    TaxiOrder ..> Driver : Использует в AssignDriver()
    DriversRepository ..> Driver : Создает
    DriversRepository ..> Car : Создает
    DriversRepository ..> PersonName : Создает
```
