# Практика: GraphViz

## 1. Описание предметной области и сущностей
*В системе реализован построитель графов в формате DOT для визуализации с помощью Graphviz. DotGraphBuilder - основной класс, предоставляющий фасад для создания ориентированных и неориентированных графов. Graph - класс, представляющий граф, содержащий коллекции узлов и ребер. GraphNode - узел графа, имеющий атрибуты. GraphEdge - ребро графа, соединяющее два узла и имеющее атрибуты. NodeBuilder - построитель узлов, предоставляющий методы для добавления новых узлов и ребер, а также конфигурации текущего узла через метод With. EdgeBuilder - построитель ребер, предоставляющий методы для добавления новых узлов и ребер, а также конфигурации текущего ребра через метод With. NodeConfigurator - класс для настройки атрибутов узла в DSL. EdgeConfigurator - класс для настройки атрибутов ребра в DSL. DotFormatWriter - класс, отвечающий за сериализацию графа в DOT формате. NodeShape - статический класс с константами для определения формы узлов.*

## 2. Диаграмма классов (Mermaid)
```mermaid
classDiagram
    direction TB

    class DotGraphBuilder {
        -Graph graph
        +DirectedGraph(string graphName)$ DotGraphBuilder
        +UndirectedGraph(string graphName)$ DotGraphBuilder
        +Build() string
        +AddNode(string nodeName) NodeBuilder
        +AddEdge(string fromNode, string toNode) EdgeBuilder
    }

    class NodeBuilder {
        -DotGraphBuilder builder
        -GraphNode node
        +AddNode(string nodeName) NodeBuilder
        +AddEdge(string from, string to) EdgeBuilder
        +Build() string
        +With(Action~NodeConfigurator~ configure) DotGraphBuilder
    }

    class EdgeBuilder {
        -DotGraphBuilder builder
        -GraphEdge edge
        +AddNode(string nodeName) NodeBuilder
        +AddEdge(string from, string to) EdgeBuilder
        +Build() string
        +With(Action~EdgeConfigurator~ configure) DotGraphBuilder
    }

    class NodeConfigurator {
        -GraphNode node
        +Color(string value) NodeConfigurator
        +FontSize(int value) NodeConfigurator
        +Label(string value) NodeConfigurator
        +Shape(string value) NodeConfigurator
    }

    class EdgeConfigurator {
        -GraphEdge edge
        +Color(string value) EdgeConfigurator
        +FontSize(int value) EdgeConfigurator
        +Label(string value) EdgeConfigurator
        +Weight(double value) EdgeConfigurator
    }

    class Graph {
        <<external>>
        +AddNode(string name) GraphNode
        +AddEdge(string from, string to) GraphEdge
    }

    class GraphNode {
        <<external>>
        +Attributes Dictionary~string, string~
    }

    class GraphEdge {
        <<external>>
        +Attributes Dictionary~string, string~
    }

    class DotFormatWriter {
        <<external>>
        +Write(Graph graph)
    }

    class NodeShape {
        +Box string$
        +Ellipse string$
    }

    DotGraphBuilder *-- Graph : graph
    Graph *-- GraphNode : Содержит узлы
    Graph *-- GraphEdge : Содержит ребра
    DotGraphBuilder ..> DotFormatWriter : Использует в Build
    NodeBuilder --> DotGraphBuilder : builder
    NodeBuilder --> GraphNode : node
    EdgeBuilder --> DotGraphBuilder : builder
    EdgeBuilder --> GraphEdge : edge
    NodeBuilder ..> NodeConfigurator : Создает в With
    EdgeBuilder ..> EdgeConfigurator : Создает в With
    NodeConfigurator --> GraphNode : node
    EdgeConfigurator --> GraphEdge : edge
```
