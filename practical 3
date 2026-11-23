"""
Practical 3: A* Heuristic Search -- AI Hospital Path Planner
Urgent medicine delivery: Pharmacy -> Emergency Ward

"""

import heapq
import math
import time

HOSPITAL_GRAPH = {
    "Pharmacy": [("Reception", 2), ("Ward B", 4)],
    "Reception": [("Pharmacy", 2), ("Laboratory", 2), ("Emergency", 4)],
    "Ward B": [("Pharmacy", 4), ("Emergency", 3), ("ICU", 3)],
    "Laboratory": [("Reception", 2), ("Ward A", 3)],
    "Emergency": [("Reception", 4), ("Ward B", 3), ("Ward A", 2)],
    "Ward A": [("Laboratory", 3), ("Emergency", 2)],
    "ICU": [("Ward B", 3)],
}

COORDINATES = {
    "Pharmacy": (0, 0),
    "Reception": (2, 0),
    "Ward B": (4, 0),
    "ICU": (5, 2),
    "Laboratory": (2, -2),
    "Emergency": (4, -2),
    "Ward A": (5, -4),
}


def h_manhattan(a, b):
    return abs(a[0] - b[0]) + abs(a[1] - b[1])


def h_euclidean(a, b):
    return math.sqrt((a[0] - b[0]) ** 2 + (a[1] - b[1]) ** 2)


def h_value(node, goal, heuristic):
    c1, c2 = COORDINATES[node], COORDINATES[goal]
    if heuristic == "zero":
        return 0.0
    if heuristic == "euclidean":
        return h_euclidean(c1, c2)
    return float(h_manhattan(c1, c2))


def a_star(start, goal, heuristic="manhattan"):
    t0 = time.perf_counter()
    counter = 0
    pq = [(h_value(start, goal, heuristic), counter, start)]
    g_score = {start: 0.0}
    parent = {start: None}
    open_set = {start}
    closed = set()
    steps = []

    while pq:
        _f, _, current = heapq.heappop(pq)
        if current not in open_set:
            continue
        open_set.remove(current)
        closed.add(current)
        g = g_score[current]
        h = h_value(current, goal, heuristic)
        steps.append((current, g, h, g + h))
        if current == goal:
            break
        for neigh, w in HOSPITAL_GRAPH.get(current, []):
            if neigh in closed:
                continue
            tg = g + w
            if neigh not in g_score or tg < g_score[neigh]:
                parent[neigh] = current
                g_score[neigh] = tg
                counter += 1
                heapq.heappush(pq, (tg + h_value(neigh, goal, heuristic), counter, neigh))
                open_set.add(neigh)

    path = []
    if goal in parent or start == goal:
        c = goal if goal in g_score else None
        while c is not None:
            path.append(c)
            c = parent.get(c)
        path.reverse()

    cost = 0
    for i in range(len(path) - 1):
        for n, w in HOSPITAL_GRAPH[path[i]]:
            if n == path[i + 1]:
                cost += w
                break

    return {
        "path": path,
        "cost": cost,
        "explored": len(closed),
        "ms": round((time.perf_counter() - t0) * 1000, 3),
        "steps": steps,
    }


def main():
    start, goal = "Pharmacy", "Emergency"
    print("=" * 70)
    print("  AI HOSPITAL PATH PLANNER")
    print("  A* Heuristic Search -- Urgent Medicine Delivery")
    print("=" * 70)
    print(f"\nStart       : {start}")
    print(f"Destination : {goal}")
    print("Formula     : f(n) = g(n) + h(n)\n")

    print(f"{'Heuristic':<12} {'Route':<45} {'Cost':>5} {'Explored':>8} {'ms':>8}")
    print("-" * 85)
    for name in ("zero", "manhattan", "euclidean"):
        r = a_star(start, goal, name)
        route = " -> ".join(r["path"]) if r["path"] else "-"
        print(f"{name:<12} {route:<45} {r['cost']:>5} {r['explored']:>8} {r['ms']:>8}")

    print("\n--- A* SEARCH PROCESS (Manhattan) ---")
    r = a_star(start, goal, "manhattan")
    for i, (node, g, h, f) in enumerate(r["steps"], 1):
        print(f"Step {i}: Current={node:<12} g={g:<5.1f} h={h:<6.2f} f={f:<6.2f}")
    print("Goal reached." if r["path"] else "No path.")
    print("\nNote: h=0 makes A* behave like UCS. A good heuristic often explores fewer nodes,")
    print("but results depend on the graph and heuristic properties.")


if __name__ == "__main__":
    main()
