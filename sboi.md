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
	    +FailureType Type
	    +DateTime Date
	    +int DeviceId
	    +Failure(FailureType type, DateTime date, int deviceId)
	    +bool IsSerious()
    }

    class Common {
	    +static int IsFailureSerious(FailureType failureType)
	    +static int IsEarlier(object[] v, int day, int month, int year)
    }

    class ReportMaker {
	    +static List~string~ FindDevicesFailedBeforeDate(DateTime date, List~Failure~ failures, List~Device~ devices)
	    +static List~string~ FindDevicesFailedBeforeDateObsolete(int day, int month, int year, int[] failureTypes, int[] deviceId, object[][] times, List~Dictionary~ devices)
    }

    class FailureType {
    }

	<<enumeration>> FailureType

    Failure ..> Common : Вызывает метод IsFailureSerious()
    ReportMaker ..> Failure : Анализирует список сбоев
    ReportMaker ..> Device : Извлекает имена устройств
	ReportMaker ..> Common : Вызывает методы IsFailureSerious() и IsEarlier()
	Failure --> FailureType : Хранит тип сбоя
```
