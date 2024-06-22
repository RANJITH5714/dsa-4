class Node:
    def __init__(self, char=None, freq=0):
        self.char = char
        self.freq = freq
        self.left = None
        self.right = None

nodes = []

def calculate_frequencies(word):
    frequencies = {}
    for char in word:
        if char not in frequencies:
            freq = word.count(char)
            frequencies[char] = freq
            nodes.append(Node(char, freq))

def build_huffman_tree():
    while len(nodes) > 1:
        nodes.sort(key=lambda x: x.freq)
        left = nodes.pop(0)
        right = nodes.pop(0)
        
  merged = Node(freq=left.freq + right.freq)
  merged.left = left
  merged.right = right
        
   nodes.append(merged)

 return nodes[0]

def generate_huffman_codes(node, current_code, codes):
    if node is None:
        return

   if node.char is not None:
        codes[node.char] = current_code

   generate_huffman_codes(node.left, current_code + '0', codes)
    generate_huffman_codes(node.right, current_code + '1', codes)

def huffman_encoding(word):
    global nodes
    nodes = []
    calculate_frequencies(word)
    root = build_huffman_tree()
    codes = {}
    generate_huffman_codes(root, '', codes)
    return codes

word = "lossless"
codes = huffman_encoding(word)
encoded_word = ''.join(codes[char] for char in word)

print("Word:", word)
print("Huffman code:", encoded_word)
print("Conversion table:", codes)

def nearest_neighbor_tsp(distances):
    n = len(distances)
    visited = [False] * n
    route = [0]
    visited[0] = True
    total_distance = 0

   for _ in range(1, n):
        last = route[-1]
        nearest = None
        min_dist = float('inf')
        for i in range(n):
            if not visited[i] and distances[last][i] < min_dist:
                min_dist = distances[last][i]
                nearest = i
        route.append(nearest)
        visited[nearest] = True
        total_distance += min_dist

   total_distance += distances[route[-1]][0]
   route.append(0)
   return route, total_distance

distances = [
    [0, 2, 2, 5, 9, 3],
    [2, 0, 4, 6, 7, 8],
    [2, 4, 0, 8, 6, 3],
    [5, 6, 8, 0, 4, 9],
    [9, 7, 6, 4, 0, 10],
    [3, 8, 3, 9, 10, 0]
]

route, total_distance = nearest_neighbor_tsp(distances)
print("Route:", route)
print("Total distance:", total_distance)

def knapsack_memoization(capacity, n):
    print(f"knapsack_memoization({n}, {capacity})")
    if memo[n][capacity] is not None:
        print(f"Using memo for ({n}, {capacity})")
        return memo[n][capacity]
    
  if n == 0 or capacity == 0:
        result = 0
    elif weights[n-1] > capacity:
        result = knapsack_memoization(capacity, n-1)
    else:
        include_item = values[n-1] + knapsack_memoization(capacity-weights[n-1], n-1)
        exclude_item = knapsack_memoization(capacity, n-1)
        result = max(include_item, exclude_item)

   memo[n][capacity] = result
   return result

values = [300, 200, 400, 500]
weights = [2, 1, 5, 3]
capacity = 10
n = len(values)

memo = [[None]*(capacity + 1) for _ in range(n + 1)]

print("\nMaximum value in Knapsack =", knapsack_memoization(capacity, n))

def knapsack_tabulation():
    n = len(values)
    tab = [[0] * (capacity + 1) for _ in range(n + 1)]

   for i in range(1, n + 1):
        for w in range(1, capacity + 1):
            if weights[i-1] <= w:
                include_item = values[i-1] + tab[i-1][w - weights[i-1]]
                exclude_item = tab[i-1][w]
                tab[i][w] = max(include_item, exclude_item)
            else:
                tab[i][w] = tab[i-1][w]

   for row in tab:
        print(row)

   items_included = []
    w = capacity
    for i in range(n, 0, -1):
        if tab[i][w] != tab[i-1][w]:
            items_included.append(i-1)
            w -= weights[i-1]

   print("\nItems included:", items_included)

   return tab[n][capacity]

values = [300, 200, 400, 500]
weights = [2, 1, 5, 3]
capacity = 10
print("\nMaximum value in Knapsack =", knapsack_tabulation())

