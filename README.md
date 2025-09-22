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
    row,col = pos
    col_map = "abcdefgh"
    return f"{col_map[col]}{8-row}"

def get_moves(is_white):
    moves = []
    for r in range(8):
        for c in range(8):
            piece = board[r][c]
            if piece == ".": 
                continue
            if is_white and piece.isupper() or (not is_white and piece.islower()):
                for rr in range(8):
                    for cc in range(8):
                        if (rr,cc)!=(r,c) and board[rr][cc]=="." or (is_white and board[rr][cc].islower()) or (not is_white and board[rr][cc].isupper()):
                            moves.append(((r,c),(rr,cc)))
    return moves

def make_move(start,end):
    r1,c1=start; r2,c2=end
    board[r2][c2]=board[r1][c1]
    board[r1][c1]="."

def game():
    white_turn=True
    print_board()
    while True:
        flat=sum(board,[])
        if "K" not in flat:
            print("Black wins! King captured.")
            break
        if "k" not in flat:
            print("White wins! King captured.")
            break

        if white_turn:
            move=input("Your move (e.g., e2 e4): ").split()
            try:
                start=notation_to_pos(move[0]); end=notation_to_pos(move[1])
                make_move(start,end)
            except:
                print("Invalid input, try again."); continue
        else:
            moves=get_moves(False)
            if not moves: 
                print("Stalemate!"); break
            start,end=random.choice(moves)
            make_move(start,end)
            print(f"AI plays {pos_to_notation(start)} {pos_to_notation(end)}")

        print_board()
        white_turn=not white_turn

game()

```
### Output:

**chess board** 

<img width="258" height="214" alt="image" src="https://github.com/user-attachments/assets/9dc95c5b-497d-4d75-89d5-7eb1ff5d7a56" />

Moving from our side 

<img width="397" height="237" alt="image" src="https://github.com/user-attachments/assets/c9df8860-b997-409a-8ba0-e089f1fab6a6" />

Ai moves

<img width="277" height="250" alt="image" src="https://github.com/user-attachments/assets/181860fa-5fcd-4d08-8a6d-a54f406d29eb" />



### Result:

Thus , the program for Developing-AI-Agent-with-PEAS- chess board was implemented and executed successfully.
