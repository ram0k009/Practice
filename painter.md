# Практика: Fractal Painter. DIP

## 1. Описание предметной области и сущностей
*В системе реализован графический редактор для генерации и сохранения фрактальных изображений с настраиваемыми параметрами отрисовки. Основными сущностями являются действия пользовательского интерфейса, реализующие команды настройки изображения, палитры и сохранения результата. MainWindow выступает главным контейнером приложения, управляющим отображением меню и области визуализации через элементы Avalonia UI. ImageSettingsAction управляет параметрами изображения, используя IImageController для пересоздания холста при изменении настроек. PaletteSettingsAction предоставляет интерфейс для настройки цветовой палитры, влияющей на визуальное представление фракталов. SaveImageAction обеспечивает сохранение сгенерированного изображения в файловую систему. ImageController инкапсулирует логику работы с графическим контекстом, включая отрисовку на элементе ImageControl и экспорт в файл. Взаимодействие между действиями и окнами организовано через Func<Window>, обеспечивающую получение текущего родительского окна для отображения диалоговых окон настроек. SettingsManager через XmlObjectSerializer и FileBlobStorage отвечает за загрузку сохраненных параметров приложения из внешнего хранилища.*

## 2. Диаграмма классов (Mermaid)
```mermaid
classDiagram
    direction TB

    class IUiAction {
        <<interface>>
        +MenuCategory Category
        +string Name
        +CanExecute(object parameter) bool
        +Execute(object parameter) void
        +CanExecuteChanged event
    }

    class IImageController {
        <<interface>>
        +RecreateImage(ImageSettings) void
        +SaveImage(string path) void
    }

    class Window {
        <<Avalonia>>
    }

    class ImageSettingsAction {
        -Func~Window~ getParentWindow
        -IImageController imageController
        -ImageSettings imageSettings
        +ImageSettingsAction(IImageController, ImageSettings, Func~Window~)
        +CanExecute(object parameter) bool
        +Execute(object parameter) void
    }

    class SaveImageAction {
        -Func~Window~ getParentWindow
        -IImageController imageController
        +SaveImageAction(IImageController, Func~Window~)
        +CanExecute(object parameter) bool
        +Execute(object parameter) void
    }

    class PaletteSettingsAction {
        -Func~Window~ getParentWindow
        -Palette colorPalette
        +PaletteSettingsAction(Palette, Func~Window~)
        +CanExecute(object parameter) bool
        +Execute(object parameter) void
    }

    class MainWindow {
        -const int MenuSize
        -Menu menu
        -ImageControl image
        +MainWindow()
        +MainWindow(IUiAction[] actions, AvaloniaImageController controller)
        -InitializeComponent() void
        -CreateSettingsManager() SettingsManager$
    }

    class SettingsForm {
        +SettingsForm(ImageSettings)
        +SettingsForm(Palette)
        +ShowDialog(Window) Task
    }

    class SettingsManager {
        +SettingsManager(XmlObjectSerializer, FileBlobStorage)
        +Load() AppSettings
    }

    class AvaloniaImageController {
        +SetControl(ImageControl) void
        +RecreateImage(ImageSettings) void
        +SaveImage(string path) void
    }

    IUiAction <|.. ImageSettingsAction
    IUiAction <|.. SaveImageAction
    IUiAction <|.. PaletteSettingsAction
    IImageController <|.. AvaloniaImageController
    Window <|-- MainWindow
    Window <|-- SettingsForm
    %% Ассоциации (хранят ссылки как поля)
    ImageSettingsAction --> Window : Хранит фабрику
    SaveImageAction --> Window : Хранит фабрику
    PaletteSettingsAction --> Window : Хранит фабрику
    ImageSettingsAction --> IImageController : Хранит ссылку
    SaveImageAction --> IImageController : Хранит ссылку
    PaletteSettingsAction --> Palette : Хранит ссылку
    MainWindow ..> IUiAction : Использует для построения меню
    MainWindow ..> AvaloniaImageController : Использует для управления изображением
    MainWindow ..> SettingsManager : Создает в методе
    MainWindow ..> ImageSettings : Использует для размеров окна
    ImageSettingsAction ..> SettingsForm : Создает в Execute()
    PaletteSettingsAction ..> SettingsForm : Создает в Execute()
    SaveImageAction ..> TopLevel : Вызывает GetTopLevel()
```
