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

    %% Наследование и реализация интерфейсов
    Entity~int~ <|-- TaxiOrder : наследует
    Entity~int~ <|-- Driver : наследует
    ValueType~Car~ <|-- Car : наследует
    ITaxiApi~TaxiOrder~ <|.. TaxiApi : реализует

    %% Ассоциация (постоянные ссылки в полях)
    TaxiApi --> DriversRepository : driverRepo
    TaxiApi --> Func~DateTime~ : currentTime

    %% Композиция (жесткая связь - объекты создаются внутри)
    TaxiOrder *-- PersonName : ClientName
    TaxiOrder *-- Address : Start
    TaxiOrder *-- Address : Destination
    Driver *-- PersonName : Name
    Driver *-- Car : Car

    %% Агрегация (мягкая связь - объект приходит извне)
    TaxiOrder o-- Driver : assignedDriver

    %% Обычная ассоциация к Enum
    TaxiOrder --> TaxiOrderStatus : Status

    %% Зависимости (использование без сохранения в полях)
    TaxiApi ..> TaxiOrder : создает
    TaxiApi ..> Address : создает для передачи
    TaxiApi ..> PersonName : создает для передачи
    TaxiApi ..> Driver : использует при назначении
    TaxiOrder ..> Driver : использует в AssignDriver()
    DriversRepository ..> Driver : создает
    DriversRepository ..> Car : создает
    DriversRepository ..> PersonName : создает