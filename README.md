# IA
#Laboratorio 1
#Adriana Girón 21201


from queue import PriorityQueue

class Node:
    def __init__(self, name, cost=0, heuristic=0):
        self.name = name
        self.cost = cost
        self.heuristic = heuristic
        self.parent = None

    def __lt__(self, other):
        return (self.cost + self.heuristic) < (other.cost + other.heuristic)

# Estructuras de datos
class FIFOQueue:
    def __init__(self):
        self.queue = []
    
    def empty(self):
        return len(self.queue) == 0
    
    def top(self):
        return self.queue[0] if not self.empty() else None
    
    def pop(self):
        return self.queue.pop(0) if not self.empty() else None
    
    def add(self, item):
        self.queue.append(item)

class LIFOQueue(FIFOQueue):
    def pop(self):
        return self.queue.pop() if not self.empty() else None

class PriorityQueueWrapper:
    def __init__(self):
        self.queue = PriorityQueue()
    
    def empty(self):
        return self.queue.empty()
    
    def top(self):
        return self.queue.queue[0] if not self.empty() else None
    
    def pop(self):
        return self.queue.get() if not self.empty() else None
    
    def add(self, item):
        self.queue.put(item)

# Grafo de ejercicios
graph = {
    "Warm-up activities": [("SR", 10), ("EB", 10), ("TM", 10), ("SM", 10)],
    "Skipping Rope": [("Db", 15), ("B", 15)],
    "Exercise bike": [("CC", 25)],
    "Tread Mill": [("PB", 20), ("IB", 20)],
    "Step Mill": [("IB", 16)],
    "Dumbbell": [("LPM", 12)],
    "Barbell": [("LPM", 10)],
    "Cable-Crossover": [("CR", 10)],
    "Pulling Bars": [("CR", 6)],
    "Incline Bench": [("HS", 20)],
    "Leg Press Machine": [("S", 11)],
    "Climbing Rope": [("S", 10)],
    "Hammer Strength": [("S", 8)]
}

heuristics = {
    "Warm-up activities": 30,
    "Skipping Rope": 25,
    "Exercise bike": 20,
    "Tread Mill": 20,
    "Step Mill": 18,
    "Dumbbell": 15,
    "Barbell": 15,
    "Cable-Crossover": 10,
    "Pulling Bars": 10,
    "Incline Bench": 12,
    "Leg Press Machine": 8,
    "Climbing Rope": 5,
    "Hammer Strength": 4,
    "Stretching": 0
}

def search_algorithm(start, goal, strategy, heuristic=False):
    open_list = strategy()
    open_list.add(Node(start, heuristic=heuristics[start] if heuristic else 0))
    closed_set = set()

    while not open_list.empty():
        current = open_list.pop()
        if current.name in closed_set:
            continue

        closed_set.add(current.name)
        
        if current.name == goal:
            path = []
            while current:
                path.append(current.name)
                current = current.parent
            return path[::-1]

        for neighbor, cost in graph.get(current.name, []):
            node = Node(neighbor, current.cost + cost, heuristics[neighbor] if heuristic else 0)
            node.parent = current
            open_list.add(node)

    return None

# Ejecución de los algoritmos
start = "Warm-up activities"
goal = "Stretching"

print("BFS:", search_algorithm(start, goal, FIFOQueue))
print("DFS:", search_algorithm(start, goal, LIFOQueue))
print("UCS:", search_algorithm(start, goal, PriorityQueueWrapper))
print("Greedy Best-First:", search_algorithm(start, goal, PriorityQueueWrapper, heuristic=True))
print("A*:", search_algorithm(start, goal, PriorityQueueWrapper, heuristic=True))
