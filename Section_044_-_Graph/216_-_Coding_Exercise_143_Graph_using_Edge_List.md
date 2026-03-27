```py
class GraphUsingEdgeList:
    """
    A simple graph representation using an edge list.
    Stores vertices in a list and edges as tuples (source, destination, weight).
    Suitable for learning but inefficient for many operations.
    """

    def __init__(self):
        # List to store all vertices (unique identifiers)
        # Using a list means membership checks (if vertex in self.V) are O(n) – not ideal for large graphs.
        self.V = []
        
        # List to store all edges. Each edge is a tuple: (source, destination, weight)
        # For undirected graphs, we would need to store each direction separately or interpret one edge as bidirectional.
        self.edges = []

    def add_vertex(self, vertex):
        """
        Add a vertex to the graph if it doesn't already exist.
        :param vertex: The vertex identifier (can be any hashable type, but here we treat as comparable)
        """
        # Check for duplicates by scanning the entire vertex list.
        # Time complexity: O(|V|) because of the linear search in a list.
        if vertex not in self.V:
            self.V.append(vertex)   # Append at the end – O(1) amortized
        else:
            # Notify the user if they try to add a duplicate.
            print(f"{vertex} already exists")
        # No return value needed; the method modifies the graph in-place.

    def add_edge(self, source, destination, weight=1):
        """
        Add a directed edge from source to destination with an optional weight.
        :param source: Start vertex
        :param destination: End vertex
        :param weight: Numeric value associated with the edge (default 1 for unweighted)
        """
        # Ensure both vertices exist in the graph before adding the edge.
        # These membership checks also scan the entire vertex list – O(|V|) each.
        if source in self.V and destination in self.V:
            # Create a tuple representing the edge.
            # In a more general implementation, we might store edges as objects or namedtuples.
            edge = (source, destination, weight)
            # Append the edge to the edge list – O(1) amortized.
            self.edges.append(edge)
        else:
            # Inform the user if one or both vertices are missing.
            # A robust implementation might automatically add missing vertices, but here we choose to error.
            print("One or both the vertices are not found")
    
    def display(self):
        """
        Print the current state of the graph: all vertices and all edges.
        """
        print("Vertices")
        # Iterate over the vertex list and print each vertex.
        # Time complexity: O(|V|)
        for vertex in self.V:
            print(f"Vertex: {vertex}")
        
        # Iterate over the edge list and unpack each tuple into source, destination, weight.
        # Time complexity: O(|E|)
        for source, destination, weight in self.edges:
            # Print a formatted edge representation.
            # Note: This output does not show the weight; you could easily add it.
            print(f"{source} ---> {destination}")
        # The method doesn't return anything; it just prints to stdout.


# ----------------------------------------------------------------------
# Example usage
# ----------------------------------------------------------------------
if __name__ == "__main__":
    # Create an instance of the graph.
    graph = GraphUsingEdgeList()

    # Add three vertices.
    graph.add_vertex(1)
    graph.add_vertex(2)
    graph.add_vertex(3)

    # Add directed edges: 1->2 and 2->3.
    # Since no weight is provided, the default weight of 1 is used.
    graph.add_edge(1, 2)
    graph.add_edge(2, 3)

    # Display the graph's vertices and edges.
    graph.display()
```