"""
Practical 2: BFS, DFS, and Uniform-Cost Search on a real-life road network.
Also includes a DLS Water Tank depth-limited search example.

"""

from collections import deque
import heapq
import time

# Real-life weighted city road network (cost = travel time / distance units)
GRAPH = {
    "Warehouse": [("Hospital", 4), ("School", 2)],
    "Hospital": [("Railway Station", 3), ("Warehouse", 4)],
    "School": [("Mall", 6), ("Market", 3), ("Warehouse", 2)],
    "Railway Station": [("Office", 5), ("Hospital", 3)],
    "Mall": [("Market", 2), ("School", 6)],
    "Office": [("Market", 4), ("Railway Station", 5)],
    "Market": [("School", 3), ("Mall", 2), ("Office", 4)],
}


def path_cost(graph, path):
    cost = 0
    for i in range(len(path) - 1):
        u, v = path[i], path[i + 1]
        for neighbor, w in graph.get(u, []):
            if neighbor == v:
                cost += w
                break
    return cost


def reconstruct(parent, start, goal):
    if goal not in parent:
        return []
    path, curr = [], goal
    while curr is not None:
        path.append(curr)
        curr = parent.get(curr)
    path.reverse()
    return path if path and path[0] == start else []


def bfs(graph, start, goal):
    t0 = time.perf_counter()
    queue = deque([start])
    visited = {start}
    parent = {start: None}
    while queue:
        current = queue.popleft()
        if current == goal:
            break
        for neighbor, _w in sorted(graph.get(current, []), key=lambda x: x[0]):
            if neighbor not in visited:
                visited.add(neighbor)
                parent[neighbor] = current
                queue.append(neighbor)
    path = reconstruct(parent, start, goal)
    return {
        "name": "BFS",
        "path": path,
        "cost": path_cost(graph, path),
        "hops": max(0, len(path) - 1),
        "explored": len(visited),
        "ms": round((time.perf_counter() - t0) * 1000, 3),
    }


def dfs(graph, start, goal):
    t0 = time.perf_counter()
    stack = [start]
    visited = set()
    parent = {start: None}
    while stack:
        current = stack.pop()
        if current in visited:
            continue
        visited.add(current)
        if current == goal:
            break
        for neighbor, _w in sorted(graph.get(current, []), key=lambda x: x[0], reverse=True):
            if neighbor not in visited:
                parent[neighbor] = current
                stack.append(neighbor)
    path = reconstruct(parent, start, goal) if goal in visited else []
    return {
        "name": "DFS",
        "path": path,
        "cost": path_cost(graph, path),
        "hops": max(0, len(path) - 1),
        "explored": len(visited),
        "ms": round((time.perf_counter() - t0) * 1000, 3),
    }


def ucs(graph, start, goal):
    t0 = time.perf_counter()
    counter = 0
    pq = [(0, counter, start, [start])]
    best = {}
    path, cost = [], 0
    while pq:
        c, _, node, p = heapq.heappop(pq)
        if node in best and best[node] <= c:
            continue
        best[node] = c
        if node == goal:
            path, cost = p, c
            break
        for neighbor, w in sorted(graph.get(node, []), key=lambda x: x[0]):
            nc = c + w
            if neighbor not in best or nc < best[neighbor]:
                counter += 1
                heapq.heappush(pq, (nc, counter, neighbor, p + [neighbor]))
    return {
        "name": "UCS",
        "path": path,
        "cost": cost,
        "hops": max(0, len(path) - 1),
        "explored": len(best),
        "ms": round((time.perf_counter() - t0) * 1000, 3),
    }


def dls_water_tank(limit=6):
    """Depth-Limited Search: measure 4L using 5L and 3L tanks."""
    start, goal_a, cap_a, cap_b = (0, 0), 4, 5, 3

    def acts(state):
        a, b = state
        return [
            ((cap_a, b), "Fill Tank A"),
            ((a, cap_b), "Fill Tank B"),
            ((0, b), "Empty Tank A"),
            ((a, 0), "Empty Tank B"),
            ((a - min(a, cap_b - b), b + min(a, cap_b - b)), "Pour A->B"),
            ((a + min(b, cap_a - a), b - min(b, cap_a - a)), "Pour B->A"),
        ]

    def dls(state, depth, path, actions, visited):
        if state[0] == goal_a:
            return path[:], actions[:]
        if depth == 0:
            return None
        for nxt, act in acts(state):
            if nxt in visited:
                continue
            visited.add(nxt)
            path.append(nxt)
            actions.append(act)
            found = dls(nxt, depth - 1, path, actions, visited)
            if found:
                return found
            path.pop()
            actions.pop()
            visited.remove(nxt)
        return None

    visited = {start}
    return dls(start, limit, [start], [], visited)


def main():
    start, goal = "Warehouse", "Market"
    print("=" * 70)
    print("  AI DELIVERY ROUTE PLANNER")
    print("  BFS  *  DFS  *  UNIFORM-COST SEARCH")
    print("=" * 70)
    print(f"\nStart       : {start}")
    print(f"Destination : {goal}\n")

    results = [bfs(GRAPH, start, goal), dfs(GRAPH, start, goal), ucs(GRAPH, start, goal)]

    print(f"{'Algorithm':<8} {'Route':<55} {'Hops':>4} {'Cost':>6} {'Explored':>8}")
    print("-" * 90)
    for r in results:
        route = " -> ".join(r["path"]) if r["path"] else "--"
        print(f"{r['name']:<8} {route:<55} {r['hops']:>4} {r['cost']:>6} {r['explored']:>8}")

    print("\n" + "=" * 70)
    print("  DLS WATER TANK EXAMPLE (Depth-Limited Search with Signal)")
    print("=" * 70)
    print("Problem: Measure exactly 4 litres using 5L (Tank A) and 3L (Tank B) tanks.")
    print("Signals: GREEN (Target Reached), AMBER (Water Valve Flow), RED (Limit/Full)\n")
    found = dls_water_tank(6)
    if found:
        states, actions = found
        print("DLS Action Plan with Telemetry Signals:")
        for i, act in enumerate(actions, 1):
            a_vol, b_vol = states[i][0], states[i][1]
            if a_vol == 4:
                sig = "[SIGNAL: GREEN - TARGET 4L ACHIEVED]"
            elif a_vol == 5 or b_vol == 3:
                sig = "[SIGNAL: RED - HIGH CAPACITY ALERT]"
            else:
                sig = "[SIGNAL: AMBER - VALVE ACTIVE]"
            print(f"  Step {i}: {act:<22} -> A={a_vol}L/5L, B={b_vol}L/3L  {sig}")
        print(f"\nFinal State: Tank A = {states[-1][0]}L, Tank B = {states[-1][1]}L.")
        print("Success: Target volume measured using Depth-Limited Search valve sequence.")
    else:
        print("No solution within depth limit.")


if __name__ == "__main__":
    main()
