```py
class GraphAdjacencyMatrix:
    """
    Represents a graph using an adjacency matrix.
    This implementation supports undirected, weighted graphs.
    Vertex indices are fixed at creation and must be integers in [0, num_vertices-1].
    Labels can be assigned to vertices but are not used for indexing.
    """

    def __init__(self, num_vertices):
        """
        Initialize the graph with a fixed number of vertex slots.

        :param num_vertices: The number of vertices (indices 0..num_vertices-1)
        """
        # Store the number of vertices; used for bounds checking and matrix dimensions.
        self.num_vertices = num_vertices

        # Create a list to hold labels for each vertex index.
        # Initially all positions are None, meaning no label assigned.
        # This list is parallel to the adjacency matrix rows/columns.
        self.vertices = [None] * num_vertices

        # Create a 2D list (matrix) of size num_vertices x num_vertices,
        # initialized with zeros. In an unweighted graph, 0 means "no edge".
        # Using list comprehension ensures each row is a distinct list (not shared).
        # For weighted graphs, zeros could be ambiguous if weight 0 is allowed;
        # then a different sentinel (like None or inf) would be needed.
        self.adj_matrix = [[0] * num_vertices for row in range(num_vertices)]

    def add_vertex(self, index, label):
        """
        Assign a label to the vertex at the given index.

        :param index: Integer index in the range [0, num_vertices-1]
        :param label: Any object (e.g., string, int) to label the vertex
        """
        # Check if the index is within the pre‑allocated range.
        # If it is, assign the label. Note: this overwrites any existing label.
        if 0 <= index < self.num_vertices:
            self.vertices[index] = label
        else:
            # Print an error message; in a robust implementation, raise an exception.
            print("Index OOB")   # OOB = Out Of Bounds

    def add_edge(self, source, destination, weight=1):
        """
        Add an undirected edge between source and destination.

        :param source: Index of the source vertex
        :param destination: Index of the destination vertex
        :param weight: Weight of the edge (default 1). For unweighted graphs, use weight=1.
        """
        # Validate that both indices are within the allowed range.
        if 0 <= source < self.num_vertices and 0 <= destination < self.num_vertices:
            # Set the edge weight in the matrix for both directions.
            # This makes the graph undirected. For directed graphs, only set
            # adj_matrix[source][destination] = weight.
            self.adj_matrix[source][destination] = weight
            self.adj_matrix[destination][source] = weight
        else:
            # Notify the user if any vertex is out of bounds.
            print("One or both vertices not found.")

    def display(self):
        """
        Print the vertex labels and the adjacency matrix.
        """
        # Iterate over vertex indices and their labels (if any).
        # enumerate yields (index, value) for each position in self.vertices.
        for index, label in enumerate(self.vertices):
            if label is not None:
                # Only print vertices that have been assigned a label.
                print(f"Vertex Index : {index}, label :{label} ")

        # Print the adjacency matrix row by row.
        # Each row is a list of integers representing edge weights.
        for row in self.adj_matrix:
            print(row)


# Example usage
if __name__ == "__main__":
    # Create a graph with 3 vertex slots (indices 0, 1, 2).
    graph = GraphAdjacencyMatrix(3)

    # Add labels to vertices. These calls succeed because indices are in range.
    graph.add_vertex(0, 'A')
    graph.add_vertex(1, 'B')
    graph.add_vertex(2, 'C')

    # This will print "Index OOB" because index 3 is out of range (max is 2).
    graph.add_vertex(3, 'D')

    # Add undirected edges. Weight defaults to 1.
    graph.add_edge(0, 1)   # Edge between A and B
    graph.add_edge(1, 2)   # Edge between B and C

    # Display the graph.
    graph.display()
```