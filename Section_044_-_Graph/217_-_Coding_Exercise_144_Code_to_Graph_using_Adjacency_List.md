```py
class GraphAdjacencyList:
    """
    Represents an undirected graph using an adjacency list.
    - Vertices are stored in a list (self.V) to preserve insertion order and for quick membership checks.
    - Adjacency information is stored in a dictionary (self.adj_list) that maps each vertex to a list of tuples (neighbor, weight).
    - This implementation assumes an undirected graph, so every edge is added to both vertices' neighbor lists.
    - Default weight is 1, making it suitable for unweighted graphs as well.
    """
    def __init__(self):
        """
        Initialize an empty graph.
        self.V: list of vertices (unique identifiers). Used for existence checks and iteration.
        self.adj_list: dictionary where each vertex maps to a list of (neighbor, weight).
        """
        # List to store all vertices in the order they were added.
        # Membership checks (if vertex in self.V) are O(n) because it's a list.
        # For very large graphs, using a set for vertices would give O(1) checks.
        self.V = []
        
        # Dictionary to hold adjacency lists.
        # Key: vertex, Value: list of tuples (neighbor, weight).
        # Using a dictionary allows O(1) average access to a vertex's neighbor list.
        self.adj_list = {}

    def add_vertex(self, vertex):
        """
        Add a vertex to the graph if it doesn't already exist.
        :param vertex: The vertex identifier (any hashable type).
        """
        # Check if the vertex already exists by scanning the vertex list.
        # Note: if vertex is a list or unhashable, this still works because 'in' uses equality.
        # But if we want to use a set for vertices, the vertex must be hashable.
        if vertex not in self.V:
            # Append to the vertex list to keep insertion order.
            self.V.append(vertex)
            # Initialize an empty neighbor list for the new vertex.
            # This ensures that later add_edges will have a place to store connections.
            self.adj_list[vertex] = []
        else:
            # Notify the user about the duplicate. In some applications you might silently ignore.
            print(f"Vertex {vertex} already exists")
        # The method doesn't return anything; it modifies the graph in-place.

    def add_edges(self, source, destination, weight=1):
        """
        Add an undirected edge between source and destination with an optional weight.
        :param source: Starting vertex (must exist)
        :param destination: Ending vertex (must exist)
        :param weight: Numeric weight for the edge (default 1)
        This method assumes an undirected graph, so it adds the edge in both directions.
        """
        # Verify that both vertices exist in the graph.
        # Both checks scan the vertex list (O(n) each). Could be optimized by using a set.
        if source in self.V and destination in self.V:
            # Add the edge from source to destination.
            # The neighbor is stored as a tuple (destination, weight) to include weight information.
            self.adj_list[source].append((destination, weight))
            # Since the graph is undirected, also add the reverse edge.
            # This ensures that traversals can go in both directions.
            self.adj_list[destination].append((source, weight))
        else:
            # Error message if either vertex is missing.
            print("One or both vertices not found.")
        # Note: This does not prevent adding multiple edges between the same vertices.
        # For a multigraph, that might be desired. To avoid duplicates, we could use a set for neighbors.
    
    def display(self):
        """
        Print the adjacency list representation of the graph.
        For each vertex, prints its list of neighbors with weights.
        """
        # Optional: Print the vertex list separately (commented out).
        # for vertex in self.V:
        #     print(f"Vertex -> {vertex}")
        
        # Iterate over the dictionary items to print each vertex and its neighbors.
        for vertex, neighbours in self.adj_list.items():
            # neighbours is a list of tuples (neighbor, weight).
            # The print will show the entire list, which includes the weight information.
            print(f"{vertex} : {neighbours}")
        # The output is not ordered in any particular way because dict iteration order matches insertion order (Python 3.7+).
        # To guarantee order, one could sort the keys before printing.

# Example usage and demonstration
if __name__ == "__main__":
    # Create a new graph instance.
    graph = GraphAdjacencyList()
    
    # Add some vertices. The order will be A, B, C, D, E as inserted.
    graph.add_vertex('A')
    graph.add_vertex('B')
    graph.add_vertex('C')
    graph.add_vertex('D')
    graph.add_vertex('E')
    
    # Add undirected edges. Each edge is stored twice (once per endpoint).
    graph.add_edges('A','B')   # Connects A and B with weight 1 (default)
    graph.add_edges('A','D')   # Connects A and D
    graph.add_edges('B','D')   # Connects B and D
    graph.add_edges('B','C')   # Connects B and C
    
    # Display the adjacency lists.
    graph.display()
    
    # Expected output (order may vary depending on insertion order):
    # A : [('B', 1), ('D', 1)]
    # B : [('A', 1), ('D', 1), ('C', 1)]
    # C : [('B', 1)]
    # D : [('A', 1), ('B', 1)]
    # E : []
```