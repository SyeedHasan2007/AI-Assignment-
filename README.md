# AI-Assignment-
# ==========================================================
# Experiment 1: Breadth-First Search (BFS)
# Real-Life Application: Finding the Minimum Number of
# Connections (Degrees of Separation) in a Social Network
# ==========================================================
from collections import deque
# ----------------------------------------------------------
# Step 1: Represent the social network as an adjacency list
# Each person (node) is connected to their direct friends
# ----------------------------------------------------------
social_network = {
 'Alice': ['Bob', 'Carol'],
 'Bob': ['Alice', 'David', 'Eve'],
 'Carol': ['Alice', 'Frank'],
 'David': ['Bob', 'Grace'],
 'Eve': ['Bob', 'Grace'],
 'Frank': ['Carol', 'Grace'],
 'Grace': ['David', 'Eve', 'Frank', 'Henry'],
 'Henry': ['Grace']
}
def bfs_shortest_path(graph, start, goal):
 """
 Performs BFS starting from 'start' node to find the
 shortest path (minimum number of connections) to 'goal'.
 Returns the path as a list and prints the traversal steps.
 """
 # visited keeps track of nodes we have already added to the queue
 visited = set([start])
 # queue holds the frontier nodes to explore (FIFO order)
 queue = deque([start])
 # parent dictionary is used to reconstruct the shortest path
 parent = {start: None}
 step = 0
 print("Starting BFS Traversal")
 print("-" * 50)
 # ----------------------------------------------------------
 # Step 2: Standard BFS loop using a queue
 # ----------------------------------------------------------
 while queue:
 step += 1
 current = queue.popleft() # remove the front person from the queue
 print(f"Step {step}: Visiting '{current}' | Queue before pop: {list(queue)}")
 # Goal check: stop as soon as we reach the target person
 if current == goal:
 print(f"\nGoal '{goal}' found!")
 break
 # Explore all direct friends (neighbours) of the current person
 for neighbour in graph[current]:
 if neighbour not in visited:
 visited.add(neighbour) # mark as seen so we don't add it twice
 parent[neighbour] = current # remember how we reached this neighbour
 queue.append(neighbour) # add neighbour to the back of the queue
 print(f" -> Added '{neighbour}' to queue")
 # ----------------------------------------------------------
 # Step 3: Reconstruct the shortest path using parent pointers
 # ----------------------------------------------------------
 path = []
 node = goal
 while node is not None:
 path.append(node)
 node = parent.get(node)
 path.reverse()
 return path
# ----------------------------------------------------------
# Step 4: Define the start person and the goal person
# ----------------------------------------------------------
start_person = 'Alice'
goal_person = 'Henry'
print(f"Social Network Graph: {social_network}\n")
print(f"Finding minimum connections between '{start_person}' and '{goal_person}'\n")
shortest_path = bfs_shortest_path(social_network, start_person, goal_person)
# ----------------------------------------------------------
# Step 5: Display the final result
# ----------------------------------------------------------
print("-" * 50)
print("RESULT")
print("-" * 50)
print("Shortest Path :", " -> ".join(shortest_path))
print("Minimum Number of Connections (Steps) :", len(shortest_path) - 1)
