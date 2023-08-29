# Codsofttask1-AI
import tkinter as tk
from tkinter import messagebox
import random

class TicTacToe:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("Tic Tac Toe")
        self.current_player = "X"
        self.board = [["" for _ in range(3)] for _ in range(3)]

        self.buttons = [[None for _ in range(3)] for _ in range(3)]
        for i in range(3):
            for j in range(3):
                self.buttons[i][j] = tk.Button(self.window, text="", font=("Helvetica", 24), width=5, height=2,
                                              command=lambda row=i, col=j: self.on_button_click(row, col))
                self.buttons[i][j].grid(row=i, column=j)

    def on_button_click(self, row, col):
        if self.board[row][col] == "" and not self.check_winner():
            self.board[row][col] = self.current_player
            self.buttons[row][col].config(text=self.current_player)
            if self.check_winner():
                messagebox.showinfo("Tic Tac Toe", f"Player {self.current_player} wins!")
                self.reset_board()
            else:
                if self.current_player == "X":
                    self.current_player = "O"
                    self.make_computer_move()
                else:
                    self.current_player = "X"

    def make_computer_move(self):
        if not self.check_winner():
            empty_cells = [(row, col) for row in range(3) for col in range(3) if self.board[row][col] == ""]
            if empty_cells:
                row, col = self.get_best_move()
                self.on_button_click(row, col)

    def get_best_move(self):
        best_score = float("-inf")
        best_move = None
        for row in range(3):
            for col in range(3):
                if self.board[row][col] == "":
                    self.board[row][col] = "O"
                    score = self.minimax(self.board, 0, False)
                    self.board[row][col] = ""
                    if score > best_score:
                        best_score = score
                        best_move = (row, col)
        return best_move

    def minimax(self, board, depth, is_maximizing):
        scores = {"X": -1, "O": 1, "tie": 0}

        if self.check_winner():
            winner = "X" if self.current_player == "O" else "O"
            return scores[winner]

        if is_maximizing:
            best_score = float("-inf")
            for row in range(3):
                for col in range(3):
                    if board[row][col] == "":
                        board[row][col] = "O"
                        score = self.minimax(board, depth + 1, False)
                        board[row][col] = ""
                        best_score = max(best_score, score)
            return best_score
        else:
            best_score = float("inf")
            for row in range(3):
                for col in range(3):
                    if board[row][col] == "":
                        board[row][col] = "X"
                        score = self.minimax(board, depth + 1, True)
                        board[row][col] = ""
                        best_score = min(best_score, score)
            return best_score

    def check_winner(self):
        for i in range(3):
            if self.board[i][0] == self.board[i][1] == self.board[i][2] != "":
                return True
            if self.board[0][i] == self.board[1][i] == self.board[2][i] != "":
                return True
        if self.board[0][0] == self.board[1][1] == self.board[2][2] != "":
            return True
        if self.board[0][2] == self.board[1][1] == self.board[2][0] != "":
            return True
        if all(self.board[i][j] != "" for i in range(3) for j in range(3)):
            messagebox.showinfo("Tic Tac Toe", "It's a tie!")
            self.reset_board()
        return False

    def reset_board(self):
        for i in range(3):
            for j in range(3):
                self.board[i][j] = ""
                self.buttons[i][j].config(text="")
        self.current_player = "X"

    def run(self):
        self.window.mainloop()

if __name__ == "__main__":
    game = TicTacToe()
    game.run()



