# Практика: Геометрия-2

## 1. Описание предметной области и сущностей
*В системе моделируются трехмерные геометрические тела. IVisitor<T> - интерфейс посетителя, позволяющий выполнять различные операции над иерархией тел без изменения их классов. Body определяет базовое тело с положением в пространстве. Ball - шар с радиусом. RectangularCuboid - прямоугольный параллелепипед с размерами по трем осям. Cylinder - цилиндр с радиусом и высотой. CompoundBody - составное тело, содержащее список других тел. BoundingBoxVisitor - посетитель, вычисляющий параллелепипед для любого тела. BoxifyVisitor - посетитель, заменяющий каждое тело его параллелепипедом. Vector3 - структура для хранения координат точки или вектора в трехмерном пространстве.*

## 2. Диаграмма классов (Mermaid)
```mermaid
classDiagram
    direction TB

    class IVisitor~T~ {
        <<interface>>
        +Visit(Ball) T
        +Visit(RectangularCuboid) T
        +Visit(Cylinder) T
        +Visit(CompoundBody) T
    }

    class Body {
        +Vector3 Position
        +ContainsPoint(Vector3) bool
        +GetBoundingBox() RectangularCuboid
        +Accept~T~(IVisitor~T~) T
    }

    class Ball {
        +double Radius
        +ContainsPoint(Vector3) bool
        +GetBoundingBox() RectangularCuboid
        +Accept~T~(IVisitor~T~) T
    }

    class RectangularCuboid {
        +double SizeX
        +double SizeY
        +double SizeZ
        +ContainsPoint(Vector3) bool
        +GetBoundingBox() RectangularCuboid
        +Accept~T~(IVisitor~T~) T
    }

    class Cylinder {
        +double SizeZ
        +double Radius
        +ContainsPoint(Vector3) bool
        +GetBoundingBox() RectangularCuboid
        +Accept~T~(IVisitor~T~) T
    }

    class CompoundBody {
        +IReadOnlyList~Body~ Parts
        +ContainsPoint(Vector3) bool
        +GetBoundingBox() RectangularCuboid
        +Accept~T~(IVisitor~T~) T
    }

    class BoundingBoxVisitor {
        +Visit(Ball) RectangularCuboid
        +Visit(RectangularCuboid) RectangularCuboid
        +Visit(Cylinder) RectangularCuboid
        +Visit(CompoundBody) RectangularCuboid
    }

    class BoxifyVisitor {
        +Visit(Ball) Body
        +Visit(RectangularCuboid) Body
        +Visit(Cylinder) Body
        +Visit(CompoundBody) Body
    }

    class Vector3 {
        +double X
        +double Y
        +double Z
    }

    IVisitor~RectangularCuboid~ <|.. BoundingBoxVisitor : Реализует
    IVisitor~Body~ <|.. BoxifyVisitor : Реализует

    Body <|-- Ball : Наследует
    Body <|-- RectangularCuboid : Наследует
    Body <|-- Cylinder : Наследует
    Body <|-- CompoundBody : Наследует

    CompoundBody o-- Body : Содержит
    Body --> Vector3 : Имеет свойство Position
    Body ..> IVisitor~T~ : Использует в Accept
```
