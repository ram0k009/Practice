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

    %% Исправлено: Билдер жестко владеет Графом, а Граф жестко владеет своими Узлами и Ребрами
    DotGraphBuilder *-- Graph : Композиция
    Graph *-- GraphNode : Композиция
    Graph *-- GraphEdge : Композиция

    DotGraphBuilder ..> DotFormatWriter : Зависимость
    
    NodeBuilder --> DotGraphBuilder : Ассоциация
    NodeBuilder --> GraphNode : Ассоциация
    
    EdgeBuilder --> DotGraphBuilder : Ассоциация
    EdgeBuilder --> GraphEdge : Ассоциация
    
    NodeBuilder ..> NodeConfigurator : Зависимость
    EdgeBuilder ..> EdgeConfigurator : Зависимость
    
    NodeConfigurator --> GraphNode : Ассоциация
    EdgeConfigurator --> GraphEdge : Ассоциация

    %% Связь DotGraphBuilder ..> NodeShape удалена, так как в коде они не связаны напрямую