import random

def print_board(board):
    for row in board:
        print(' '.join(str(num) if num != 0 else '.' for num in row))

def is_valid(board, row, col, num):
    # Check the row
    if num in board[row]:
        return False
    # Check the column
    if num in (board[r][col] for r in range(9)):
        return False
    # Check the 3x3 box
    box_row, box_col = 3 * (row // 3), 3 * (col // 3)
    for r in range(box_row, box_row + 3):
        for c in range(box_col, box_col + 3):
            if board[r][c] == num:
                return False
    return True

def find_empty(board):
    for row in range(9):
        for col in range(9):
            if board[row][col] == 0:
                return row, col
    return None

def solve(board):
    empty = find_empty(board)
    if not empty:
        return True  # Solved
    row, col = empty
    for num in range(1, 10):
        if is_valid(board, row, col, num):
            board[row][col] = num
            if solve(board):
                return True
            board[row][col] = 0
    return False

def generate_board():
    board = [[0] * 9 for _ in range(9)]
    for _ in range(17):  # Fill the board with a few numbers to start
        row, col = random.randint(0, 8), random.randint(0, 8)
        num = random.randint(1, 9)
        if is_valid(board, row, col, num):
            board[row][col] = num
    return board

def main():
    board = generate_board()
    print("Initial Sudoku Board:")
    print_board(board)
    
    if solve(board):
        print("\nSolved Sudoku Board:")
        print_board(board)
    else:
        print("\nNo solution exists.")

if __name__ == "__main__":
    main()
