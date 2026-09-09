# AI-Assignment-
1) BFS
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


------------------------------------------------------------------------------------------------------------------------------------------


2) A* Algorithm
# ==========================================================
# Experiment 2: A* (A-Star) Search Algorithm
# Real-Life Application: GPS Navigation / Road Route Finding
# ==========================================================
# ----------------------------------------------------------
# Step 1: Represent the road network as a weighted graph
# Numbers on each edge represent the road distance (in km)
# between two cities.
# ----------------------------------------------------------
road_network = {
 'Chennai': {'Vellore': 4, 'Kanchipuram': 2},
 'Vellore': {'Chennai': 4, 'Kanchipuram': 1, 'Salem': 5},
 'Kanchipuram': {'Chennai': 2, 'Vellore': 1, 'Krishnagiri': 7},
 'Salem': {'Vellore': 5, 'Krishnagiri': 3, 'Bangalore': 6},
 'Krishnagiri': {'Kanchipuram': 7, 'Salem': 3, 'Bangalore': 2},
 'Bangalore': {'Salem': 6, 'Krishnagiri': 2}
}
# ----------------------------------------------------------
# Step 2: Heuristic values h(n) = estimated straight-line
# distance (in km) from each city to the destination (Bangalore)
# NOTE: These values are chosen so that they never OVERESTIMATE
# the true remaining distance -> this makes the heuristic
# "admissible", which is required for A* to guarantee an
# optimal (shortest) path.
# ----------------------------------------------------------
heuristic = {
 'Chennai': 7,
 'Vellore': 6,
 'Kanchipuram': 6,
 'Salem': 3,
 'Krishnagiri': 1,
 'Bangalore': 0
}
def a_star_search(graph, heuristic, start, goal):
 """
 Performs A* search from 'start' to 'goal'.
 f(n) = g(n) + h(n)
 g(n) -> actual cost travelled so far from start to node n
 h(n) -> estimated (heuristic) cost from node n to goal
 f(n) -> estimated total cost of the cheapest path through n
 A* always expands the node with the lowest f(n) value next.
 Returns the optimal path and its total cost.
 """
 # open_list stores nodes yet to be explored: each entry is (node, g_cost)
 open_list = [(start, 0)]
 # g_cost keeps the best known cost from start to each node
 g_cost = {start: 0}
 # parent is used to reconstruct the final path
 parent = {start: None}
 # closed_set stores nodes that are already fully processed
 closed_set = set()
 step = 0
 print("Starting A* Search")
 print("-" * 70)
 while open_list:
 step += 1
 # ------------------------------------------------------
 # Step 3: Pick the node in open_list with smallest f(n)
 # (simple linear search - easy to visualise in PyTutor)
 # ------------------------------------------------------
 best_index = 0
 best_node, best_g = open_list[0]
 best_f = best_g + heuristic[best_node]
 for i in range(1, len(open_list)):
 node, g_val = open_list[i]
 f_value = g_val + heuristic[node]
 if f_value < best_f:
 best_f = f_value
 best_index = i
 current, current_g = open_list.pop(best_index)
 # Skip this entry if the node was already finalised earlier
 # through a cheaper path (this can happen because we use a
 # simple list instead of a priority queue with decrease-key)
 if current in closed_set:
 step -= 1
 continue
 print(f"Step {step}: Expanding '{current}' "
 f"| g={current_g} h={heuristic[current]} f={current_g + 
heuristic[current]}")
 # Goal check
 if current == goal:
 print(f"\nGoal '{goal}' reached!")
 break
 closed_set.add(current)
 # ------------------------------------------------------
 # Step 4: Explore neighbours and update costs if a
 # cheaper path is found
 # ------------------------------------------------------
 for neighbour, edge_cost in graph[current].items():
 if neighbour in closed_set:
 continue
 new_g = current_g + edge_cost
 # If neighbour is new OR we found a cheaper path to it
 if neighbour not in g_cost or new_g < g_cost[neighbour]:
 g_cost[neighbour] = new_g
 parent[neighbour] = current
 open_list.append((neighbour, new_g))
 print(f" -> Updated '{neighbour}': "
 f"g={new_g} h={heuristic[neighbour]} f={new_g + 
heuristic[neighbour]}")
 # ----------------------------------------------------------
 # Step 5: Reconstruct the optimal path using parent pointers
 # ----------------------------------------------------------
 path = []
 node = goal
 while node is not None:
 path.append(node)
 node = parent[node]
 path.reverse()
 return path, g_cost[goal]
# ----------------------------------------------------------
# Step 6: Define start city and destination city
# ----------------------------------------------------------
start_city = 'Chennai'
goal_city = 'Bangalore'
print(f"Road Network: {road_network}\n")
print(f"Finding optimal route from '{start_city}' to '{goal_city}'\n")
optimal_path, total_cost = a_star_search(road_network, heuristic, start_city, 
goal_city)
# ----------------------------------------------------------
# Step 7: Display the final result
# ----------------------------------------------------------
print("-" * 70)
print("RESULT")
print("-" * 70)
print("Optimal Route :", " -> ".join(optimal_path))
print("Total Route Cost :", total_cost, "km")
