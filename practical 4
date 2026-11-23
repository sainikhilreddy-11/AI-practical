"""
Practical 4: AI Game Arena -- Minimax & Alpha-Beta
Games: Tic-Tac-Toe, Connect Four (depth-limited), Nim

"""

import sys
from pathlib import Path

ROOT = Path(__file__).resolve().parents[2]
if str(ROOT) not in sys.path:
    sys.path.insert(0, str(ROOT))

from practicals.practical_04_minimax.logic import (
    get_ai_decision,
    connect4_ai_move,
    c4_empty_board,
    c4_drop,
    nim_ai_move,
)


def demo_ttt():
    print("=" * 60)
    print("  TIC-TAC-TOE  (legacy Human=X, AI=O for console demo)")
    print("=" * 60)
    board = ["X", "", "", "", "", "", "", "", ""]
    result = get_ai_decision(board, algorithm="alphabeta", difficulty="hard")
    print("Board after AI:", result["board"])
    print("AI move index :", result["best_move"])
    s = result["stats"]
    print(f"Algorithm     : {s['algorithm']}")
    print(f"Nodes (MM)    : {s['pure_minimax_nodes']}")
    print(f"Nodes (AB)    : {s['alpha_beta_nodes']}")
    print(f"Saved         : {s['computation_saved_pct']}%")
    print(f"Thinking      : {s['thinking_sec']} s")


def demo_connect4():
    print("\n" + "=" * 60)
    print("  CONNECT FOUR  (Human=R, AI=Y, depth-limited)")
    print("=" * 60)
    board = c4_empty_board()
    board, _ = c4_drop(board, 3, "R")
    result = connect4_ai_move(board, algorithm="alphabeta", difficulty="medium")
    print("AI column    :", result["best_move"])
    print("Winner       :", result["winner"])
    s = result["stats"]
    print(f"Algorithm    : {s['algorithm']}")
    print(f"Depth        : {s['search_depth']}")
    print(f"Nodes        : {s['moves_evaluated']}")
    print(f"Thinking     : {s['thinking_sec']} s")


def demo_nim():
    print("\n" + "=" * 60)
    print("  NIM  (stones, remove 1-3, last stone wins)")
    print("=" * 60)
    stones = 7
    print("Stones remaining:", stones)
    result = nim_ai_move(stones, algorithm="alphabeta", difficulty="hard")
    print("AI takes     :", result["best_move"])
    print("Stones left  :", result["stones"])
    s = result["stats"]
    print(f"Nodes        : {s['moves_evaluated']}")
    print("Flow: State -> Actions -> Future states -> Minimax -> Best action")


if __name__ == "__main__":
    demo_ttt()
    demo_connect4()
    demo_nim()
    print("\nDone. Use the web AI Game Arena for interactive play.")
