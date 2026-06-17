classDiagram
    direction TB

    %% Интерфейсы
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

    %% Классы
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

    %% Реализация интерфейсов
    IUiAction <|.. ImageSettingsAction
    IUiAction <|.. SaveImageAction
    IUiAction <|.. PaletteSettingsAction
    IImageController <|.. AvaloniaImageController

    %% Наследование
    Window <|-- MainWindow
    Window <|-- SettingsForm

    %% Ассоциации (хранят ссылки как поля)
    ImageSettingsAction --> Window : Хранит фабрику
    SaveImageAction --> Window : Хранит фабрику
    PaletteSettingsAction --> Window : Хранит фабрику
    
    ImageSettingsAction --> IImageController : Хранит ссылку
    SaveImageAction --> IImageController : Хранит ссылку
    PaletteSettingsAction --> Palette : Хранит ссылку

    %% Зависимости (временное использование)
    MainWindow ..> IUiAction : Использует для построения меню
    MainWindow ..> AvaloniaImageController : Использует для управления изображением
    MainWindow ..> SettingsManager : Создает в методе
    MainWindow ..> ImageSettings : Использует для размеров окна
    
    ImageSettingsAction ..> SettingsForm : Создает в Execute()
    PaletteSettingsAction ..> SettingsForm : Создает в Execute()
    SaveImageAction ..> TopLevel : Вызывает GetTopLevel()