# Практика: Сбои

## 1. Описание предметной области и сущностей
*В системе хранится информация об устройствах и зафиксированных на них сбоях. Device - отвечает за хранение информации об устройстве. Failure - фиксирует сбой. FailureType - тип сбоев. Common - класс, содержащий методы для общих операций. ReportMaker - основной класс для генерации отчетов. ReportMaker берет список сбоев и список устройств. Он проверяет каждый сбой, если сбой серьезный и случился до нужной даты, то ReportMaker находит устройство по его номеру и запоминает название этого устройства.*

## 2. Диаграмма классов (Mermaid)
```mermaid
classDiagram
direction TB
    class Device {
        +int Id
        +string Name
        +Device(int id, string name)
    }

    class Failure {
        -FailureType Type
        -DateTime Date
        -Device Device
        +Failure(FailureType type, DateTime date, Device device)
        +bool IsSerious()
        +bool IsEarlierThan(DateTime date)
        +Device GetDevice()
    }

    class Common {
        +static int IsFailureSerious(int failureType)
        +static int Earlier(object[] time, int day, int month, int year)
    }

    class ReportMaker {
        +static List~string~ FindDevicesFailedBeforeDate(DateTime date, Failure[] failures)
        +static List~string~ FindDevicesFailedBeforeDateObsolete(int day, int month, int year, int[] failureTypes, int[] deviceId, object[][] times, List~Dictionary~ devices)
    }

    class FailureType {
        <<enumeration>>
        Type1
        Type2
        Type3
        Type4
    }

    Failure --> Device : Хранит устройство
    Failure --> FailureType : Хранит тип сбоя
    ReportMaker ..> Failure : Получает данные о сбоях
    ReportMaker ..> Common : Вызывает методы IsFailureSerious() и Earlier()
```
