# eco-trail

Creating a smart waste management system that optimizes garbage collection routes and schedules involves several components. Since we are working within a text-based interface and cannot deploy complex systems, I'll provide a simplified Python program illustrating the basic idea. We'll use basic data to simulate routes and schedules. Integrating real systems would require additional hardware interfaces, databases, and GIS data, which are outside this scope.

To build this program, we'll use the `networkx` library for route optimization. Please ensure you have it installed using `pip install networkx`.

Here's a basic outline of the program:

```python
import networkx as nx
import matplotlib.pyplot as plt

def create_graph():
    """
    Create a simple graph representing the city map for garbage collection.
    Nodes represent waste collection points, and edges represent roads between them.
    Weights on edges represent distance.
    """
    G = nx.Graph()

    # Add nodes
    collection_points = range(1, 6)  # Let's assume we have 5 collection points
    for point in collection_points:
        G.add_node(point)

    # Add edges with weights (distances)
    edges = [
        (1, 2, 7),
        (2, 3, 10),
        (3, 4, 5),
        (4, 5, 2),
        (1, 5, 8),
        (2, 5, 3)
    ]

    G.add_weighted_edges_from(edges)

    return G

def find_shortest_path(G, start, end):
    """
    Find the shortest path in the graph from start node to end node using Dijkstra's algorithm.
    Returns the path and the path length.
    """
    try:
        path = nx.dijkstra_path(G, start, end)
        path_length = nx.dijkstra_path_length(G, start, end)
        return path, path_length
    except nx.NetworkXNoPath:
        raise ValueError(f"No path exists between {start} and {end}.")
    except (nx.NodeNotFound, nx.NetworkXError) as e:
        raise ValueError(f"Encountered an error with the graph: {str(e)}")

def main():
    """
    Main function that creates the city graph, calculates the optimal route
    for waste collection, and displays the results.
    """
    # Step 1: Create the graph
    G = create_graph()

    # Display the created graph using matplotlib
    pos = nx.spring_layout(G)
    nx.draw(G, pos, with_labels=True, node_color='lightblue', node_size=700, font_size=12)
    labels = nx.get_edge_attributes(G, 'weight')
    nx.draw_networkx_edge_labels(G, pos, edge_labels=labels, font_size=10)
    plt.title("City Map for Garbage Collection")
    plt.show()

    # Step 2: Define collection start and end points (could be the same for a round trip)
    start_point = 1
    collection_end_point = 4

    # Step 3: Find the optimal route
    try:
        optimal_route, route_length = find_shortest_path(G, start_point, collection_end_point)

        print(f"Optimal route from point {start_point} to {collection_end_point}: {optimal_route}")
        print(f"Total distance of the route: {route_length} units")

    except ValueError as error:
        print(f"Error in finding the optimal route: {error}")

if __name__ == "__main__":
    main()
```

### Key Points:
- **Graph Creation**: The function `create_graph` creates a simple graph with nodes and weighted edges representing collection points and distances.
- **Route Calculation**: The `find_shortest_path` function utilizes Dijkstra’s algorithm to find the shortest path between two nodes.
- **Graph Visualization**: Uses matplotlib to visualize the graph.
- **Error Handling**: Handles potential errors such as missing nodes or no path available.

### Further Considerations:
- **Real-World Data**: For practical applications, real geographic data and possibly sensors would be necessary.
- **Route Adjustment**: The system could integrate real-time data to adjust routes dynamically based on traffic and bin fill levels.
- **Database Integration**: Data storage for routes, schedules, and sensor data in databases like PostgreSQL or MongoDB would be useful.