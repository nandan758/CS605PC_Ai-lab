goal = (1, 2, 3, 4, 5, 6, 7, 8, 0)
def manhattan(state):
    dist = 0
    for i in range(9):
        if state[i] != 0:
            goal_pos = goal.index(state[i])
            dist += abs(i // 3 - goal_pos // 3) + abs(i % 3 - goal_pos % 3)
    return dist

def get_neighbors(state):
    neighbors = []
    idx = state.index(0)
    moves = [-3, 3, -1, 1]
    for m in moves:
        new_idx = idx + m
        if 0 <= new_idx < 9:
            if idx % 3 == 2 and m == 1:
                continue
            if idx % 3 == 0 and m == -1:
                continue
            new_state = list(state)
            new_state[idx], new_state[new_idx] = new_state[new_idx], new_state[idx]
            neighbors.append(tuple(new_state))         
    return neighbors

def astar(start):
    open_list = [(manhattan(start), 0, start)]
    visited = set()
    print("Searching...")
    while open_list:
        current_node = min(open_list, key=lambda x: x[0])
        open_list.remove(current_node)
        f, g, state = current_node
        if state == goal:
            print("\nSuccess! Puzzle Solved.")
            print("Final State:", state)
            return
        if state in visited:
            continue
        visited.add(state)
        for n in get_neighbors(state):
            if n not in visited:
                g_new = g + 1
                f_new = g_new + manhattan(n)
                open_list.append((f_new, g_new, n))
    print("No solution found.")
start_state = (1, 2, 3, 4, 0, 5, 6, 7, 8)
astar(start_state)
