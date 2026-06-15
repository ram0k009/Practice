# Практика: Генератор отчетов

## 1. Описание предметной области и сущностей
*В системе моделируется генерация отчетов о погодных измерениях. IReportRenderer - интерфейс для форматирования отчета, определяющий методы создания заголовка, начала списка, элемента списка и конца списка. IStatisticsCalculator - интерфейс для вычисления статистических показателей, позволяющий получить название статистики и рассчитать ее по набору данных. HtmlRenderer - рендерер, который форматирует отчет в HTML с тегами h1, ul и li. MarkdownRenderer - рендерер, который форматирует отчет в Markdown с заголовками второго уровня и маркированными списками. MeanAndStdCalculator - калькулятор, вычисляющий среднее арифметическое и стандартное отклонение выборки. MedianCalculator - калькулятор, вычисляющий медиану выборки. ReportMaker - класс, который комбинирует рендерер и калькулятор для создания готового отчета. ReportMakerHelper - статический вспомогательный класс, предоставляющий методы для создания конкретных типов отчетов. Measurement - сущность, хранящая результаты одного измерения: температуру и влажность воздуха.*

## 2. Диаграмма классов (Mermaid)
```mermaid
classDiagram
    direction TB

    class IReportRenderer {
        <<interface>>
        +MakeCaption(string) string
        +BeginList() string
        +MakeItem(string, string) string
        +EndList() string
    }

    class IStatisticsCalculator {
        <<interface>>
        +Calculate(IEnumerable~double~) object
        +GetCaption() string
    }

    class HtmlRenderer {
        +MakeCaption(string) string
        +BeginList() string
        +EndList() string
        +MakeItem(string, string) string
    }

    class MarkdownRenderer {
        +MakeCaption(string) string
        +BeginList() string
        +EndList() string
        +MakeItem(string, string) string
    }

    class MeanAndStdCalculator {
        +GetCaption() string
        +Calculate(IEnumerable~double~) object
    }

    class MedianCalculator {
        +GetCaption() string
        +Calculate(IEnumerable~double~) object
    }

    class ReportMaker {
        -IReportRenderer renderer
        -IStatisticsCalculator calculator
        +ReportMaker(IReportRenderer, IStatisticsCalculator)
        +MakeReport(IEnumerable~Measurement~) string
    }

    class ReportMakerHelper {
        <<static>>
        +MeanAndStdHtmlReport(IEnumerable~Measurement~) string
        +MedianMarkdownReport(IEnumerable~Measurement~) string
        +MeanAndStdMarkdownReport(IEnumerable~Measurement~) string
        +MedianHtmlReport(IEnumerable~Measurement~) string
    }

    class Measurement {
        +double Temperature
        +double Humidity
    }

    class MeanAndStd {
        +double Mean
        +double Std
    }

    IReportRenderer <|.. HtmlRenderer : Реализует
    IReportRenderer <|.. MarkdownRenderer : Реализует
    IStatisticsCalculator <|.. MeanAndStdCalculator : Реализует
    IStatisticsCalculator <|.. MedianCalculator : Реализует
    ReportMaker o-- IReportRenderer : Хранит рендер
    ReportMaker o-- IStatisticsCalculator : Хранит калькулятор
    ReportMaker ..> Measurement : Принимает в MakeReport
    MeanAndStdCalculator ..> MeanAndStd : Создает внутри Calculate
    ReportMakerHelper ..> ReportMaker : Создает 
    ReportMakerHelper ..> HtmlRenderer : Создает 
    ReportMakerHelper ..> MarkdownRenderer : Создает 
    ReportMakerHelper ..> MeanAndStdCalculator : Создает 
    ReportMakerHelper ..> MedianCalculator : Создает 
    ReportMakerHelper ..> Measurement : Передает в методы
```
