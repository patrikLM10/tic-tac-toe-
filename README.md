# tic-tac-toe-
I've created a Tic-Tac-Toe game implemented using the Minimax algorithm. This algorithm is a decision-making process that explores all possible game states to determine the best move for a player. In the context of Tic-Tac-Toe, the Minimax algorithm helps the computer player make optimal moves, ensuring that it either wins or draws the game
import tkinter as tk
from tkinter import messagebox
import random

class TicTacToe(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Tic Tac Toe - JISHNU")
        self.geometry("300x300")
        self.board = [[' ' for _ in range(3)] for _ in range(3)]
        self.buttons = [[None for _ in range(3)] for _ in range(3)]
        self.current_player = 'X'
        self.create_board()

    def create_board(self):
        for i in range(3):
            for j in range(3):
                button = tk.Button(self, text='', font=('Helvetica', 24), width=3, height=1,
                                   command=lambda row=i, col=j: self.make_move(row, col))
                button.grid(row=i, column=j, padx=5, pady=5)
                self.buttons[i][j] = button

    def make_move(self, row, col):
        if self.board[row][col] == ' ':
            self.board[row][col] = self.current_player
            self.buttons[row][col].config(text=self.current_player)
            if self.check_winner():
                messagebox.showinfo("Winner", f"Player {self.current_player} wins!")
                self.reset_game()
            elif all(self.board[i][j] != ' ' for i in range(3) for j in range(3)):
                messagebox.showinfo("Draw", "It's a draw! You ugly moron :(  ")
                self.reset_game()
            else:
                self.current_player = 'O' if self.current_player == 'X' else 'X'
                if self.current_player == 'O':
                    self.computer_move()

    def computer_move(self):
        best_score = -float('inf')
        best_move = None
        for i in range(3):
            for j in range(3):
                if self.board[i][j] == ' ':
                    self.board[i][j] = 'O'
                    score = self.minimax(self.board, 0, False)
                    self.board[i][j] = ' '
                    if score > best_score:
                        best_score = score
                        best_move = (i, j)
        if best_move:
            self.board[best_move[0]][best_move[1]] = 'O'
            self.buttons[best_move[0]][best_move[1]].config(text='O')
            if self.check_winner():
                messagebox.showinfo("Winner", "hey you stinky cunt u lose the game, HAHA..")
                self.reset_game()
            elif all(self.board[i][j] != ' ' for i in range(3) for j in range(3)):
                messagebox.showinfo("Draw", "It's a draw!")
                self.reset_game()
            else:
                self.current_player = 'X'

    def minimax(self, board, depth, is_maximizing):
        if self.check_winner() == 'X':
            return -10
        elif self.check_winner() == 'O':
            return 10
        elif all(board[i][j] != ' ' for i in range(3) for j in range(3)):
            return 0

        if is_maximizing:
            max_eval = -float('inf')
            for i in range(3):
                for j in range(3):
                    if board[i][j] == ' ':
                        board[i][j] = 'O'
                        eval = self.minimax(board, depth + 1, False)
                        board[i][j] = ' '
                        max_eval = max(max_eval, eval)
            return max_eval
        else:
            min_eval = float('inf')
            for i in range(3):
                for j in range(3):
                    if board[i][j] == ' ':
                        board[i][j] = 'X'
                        eval = self.minimax(board, depth + 1, True)
                        board[i][j] = ' '
                        min_eval = min(min_eval, eval)
            return min_eval

    def check_winner(self):
        for row in self.board:
            if row[0] == row[1] == row[2] != ' ':
                return row[0]
        for col in range(3):
            if self.board[0][col] == self.board[1][col] == self.board[2][col] != ' ':
                return self.board[0][col]
        if self.board[0][0] == self.board[1][1] == self.board[2][2] != ' ':
            return self.board[0][0]
        if self.board[0][2] == self.board[1][1] == self.board[2][0] != ' ':
            return self.board[0][2]
        return None

    def reset_game(self):
        for i in range(3):
            for j in range(3):
                self.board[i][j] = ' '
                self.buttons[i][j].config(text='')
        self.current_player = 'X'
        
if __name__ == "__main__":
    game = TicTacToe()
    game.mainloop()
