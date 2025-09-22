# Ex-1-Developing-AI-Agent-with-PEAS-Description
### Name: Mahesh N

### Register Number: 2305001017

### Aim:
To find the PEAS description for the given AI problem and develop an AI agent.

### Theory :
PEAS stands for:
'''
P-Performance measure

E-Environment

A-Actuators

S-Sensors
'''

It’s a framework used to define the task environment for an AI agent clearly.

### Pick an AI Problem

```

1. Self-driving car

2. Chess playing agent

3. Vacuum cleaning robot

4. Email spam filter

5. Personal assistant (like Siri or Alexa)
```

### Chess playing agent
### Algorithm:
**Step 1:** Initialize

Set up chess board in starting position.

Set agent’s side (White or Black).

Define evaluation function (to measure how good a board position is).

Set search depth (how many moves ahead to look).

**Step 2 :** Game Loop (repeat until game ends)

a. Sense/Perceive:

Observe current board state.

Generate all possible legal moves.

b. Think/Decide:

For each possible move:

Simulate move on a copy of the board.

Use Minimax with Alpha-Beta Pruning to explore opponent’s responses up to the chosen depth.

Evaluate resulting positions using the evaluation function.

Select the move with the best evaluation (maximize if agent’s turn, minimize if opponent’s).

c. Act:

Make the selected move on the real board.

d. Print/Debug (optional):

Show chosen move, current board state, and evaluation score.

**Step 3:** Stop when game ends (Checkmate, Stalemate, or Draw).

**Step 4:**
Print result (Win, Lose, Draw).

Print number of moves played.

### Program:
```
import random

# Starting chess board
board = [
    ["r","n","b","q","k","b","n","r"],
    ["p","p","p","p","p","p","p","p"],
    [".",".",".",".",".",".",".","."],
    [".",".",".",".",".",".",".","."],
    [".",".",".",".",".",".",".","."],
    [".",".",".",".",".",".",".","."],
    ["P","P","P","P","P","P","P","P"],
    ["R","N","B","Q","K","B","N","R"]
]

def print_board():
    print("   a b c d e f g h")
    for i, row in enumerate(board):
        print(f"{8-i}  " + " ".join(row))
    print()

def notation_to_pos(move):
    col_map = "abcdefgh"
    return 8-int(move[1]), col_map.index(move[0])

def pos_to_notation(pos):
    row, col = pos
    col_map = "abcdefgh"
    return f"{col_map[col]}{8-row}"

def is_in_board(r, c):
    return 0 <= r < 8 and 0 <= c < 8

def pawn_moves(r, c, is_white):
    moves = []
    direction = -1 if is_white else 1
    start_row = 6 if is_white else 1
    # move forward
    if is_in_board(r + direction, c) and board[r + direction][c] == ".":
        moves.append((r + direction, c))
        if r == start_row and board[r + 2*direction][c] == ".":
            moves.append((r + 2*direction, c))
    # capture
    for dc in [-1, 1]:
        nr, nc = r + direction, c + dc
        if is_in_board(nr, nc):
            target = board[nr][nc]
            if target != "." and target.isupper() != is_white:
                moves.append((nr, nc))
    return moves

def knight_moves(r, c, is_white):
    moves = []
    for dr, dc in [(-2,-1), (-2,1), (-1,-2), (-1,2), (1,-2), (1,2), (2,-1), (2,1)]:
        nr, nc = r + dr, c + dc
        if is_in_board(nr, nc):
            target = board[nr][nc]
            if target == "." or target.isupper() != is_white:
                moves.append((nr, nc))
    return moves

def straight_moves(r, c, is_white, directions):
    moves = []
    for dr, dc in directions:
        nr, nc = r + dr, c + dc
        while is_in_board(nr, nc):
            target = board[nr][nc]
            if target == ".":
                moves.append((nr, nc))
            else:
                if target.isupper() != is_white:
                    moves.append((nr, nc))
                break
            nr += dr
            nc += dc
    return moves

def get_piece_moves(r, c):
    piece = board[r][c]
    is_white = piece.isupper()
    piece = piece.lower()
    if piece == "p":
        return pawn_moves(r, c, is_white)
    elif piece == "n":
        return knight_moves(r, c, is_white)
    elif piece == "b":
        return straight_moves(r, c, is_white, [(-1,-1), (-1,1), (1,-1), (1,1)])
    elif piece == "r":
        return straight_moves(r, c, is_white, [(-1,0), (1,0), (0,-1), (0,1)])
    elif piece == "q":
        return straight_moves(r, c, is_white, [(-1,0), (1,0), (0,-1), (0,1), (-1,-1), (-1,1), (1,-1), (1,1)])
    elif piece == "k":
        moves = []
        for dr in [-1,0,1]:
            for dc in [-1,0,1]:
                if dr == 0 and dc == 0:
                    continue
                nr, nc = r+dr, c+dc
                if is_in_board(nr,nc):
                    target = board[nr][nc]
                    if target == "." or target.isupper() != is_white:
                        moves.append((nr,nc))
        return moves
    return []

def get_moves(is_white):
    moves = []
    for r in range(8):
        for c in range(8):
            piece = board[r][c]
            if piece == ".":
                continue
            if (is_white and piece.isupper()) or (not is_white and piece.islower()):
                for end in get_piece_moves(r, c):
                    moves.append(((r,c), end))
    return moves

def make_move(start, end):
    r1,c1 = start
    r2,c2 = end
    board[r2][c2] = board[r1][c1]
    board[r1][c1] = "."

def game():
    white_turn = True
    print_board()
    while True:
        flat = sum(board, [])
        if "K" not in flat:
            print("Black wins! King captured.")
            break
        if "k" not in flat:
            print("White wins! King captured.")
            break

        if white_turn:
            move = input("Your move (e.g., e2 e4): ").split()
            if len(move) != 2:
                print("Invalid input, try again.")
                continue
            try:
                start = notation_to_pos(move[0])
                end = notation_to_pos(move[1])
                if end not in get_piece_moves(*start):
                    print("Illegal move, try again.")
                    continue
                make_move(start, end)
            except:
                print("Invalid move, try again.")
                continue
        else:
            moves = get_moves(False)
            if not moves:
                print("Stalemate!")
                break
            start, end = random.choice(moves)
            make_move(start, end)
            print(f"AI plays {pos_to_notation(start)} {pos_to_notation(end)}")

        print_board()
        white_turn = not white_turn

game()

```
### Output:

**chess board** 

<img width="255" height="221" alt="image" src="https://github.com/user-attachments/assets/47e71a72-0634-4517-b7d5-84da3096ceb7" />


Moving from our side 

<img width="277" height="220" alt="image" src="https://github.com/user-attachments/assets/6e5f6801-b376-4be1-963d-5253ef56683e" />


Ai moves

<img width="213" height="230" alt="image" src="https://github.com/user-attachments/assets/26746761-4151-4967-90db-8c15893cc6cb" />



### Result:

Thus , the program for Developing-AI-Agent-with-PEAS- chess board was implemented and executed successfully.
