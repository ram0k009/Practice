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

    %% 1. Реализация интерфейсов
    IReportRenderer <|.. HtmlRenderer : Реализует
    IReportRenderer <|.. MarkdownRenderer : Реализует
    IStatisticsCalculator <|.. MeanAndStdCalculator : Реализует
    IStatisticsCalculator <|.. MedianCalculator : Реализует

    %% 2. Агрегация (объекты передаются в конструктор извне)
    ReportMaker o-- IReportRenderer : Хранит рендерер
    ReportMaker o-- IStatisticsCalculator : Хранит калькулятор

    %% 3. Зависимости (использование типов в сигнатурах методов или локально)
    ReportMaker ..> Measurement : Принимает в MakeReport
    MeanAndStdCalculator ..> MeanAndStd : Создает внутри Calculate
    
    ReportMakerHelper ..> ReportMaker : Создает локально
    ReportMakerHelper ..> HtmlRenderer : Создает локально
    ReportMakerHelper ..> MarkdownRenderer : Создает локально
    ReportMakerHelper ..> MeanAndStdCalculator : Создает локально
    ReportMakerHelper ..> MedianCalculator : Создает локально
    ReportMakerHelper ..> Measurement : Передает в методы