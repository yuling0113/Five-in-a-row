# Gomoku Game
A classic Gomoku (Five in a Row) game based on Python and Tkinter commands is displayed with special function including changeable username, undo options, score recording, and a visual interface.
## Features

**Rule Reading Selection**: Players can choose whether skip the rule reading.

**Customizable Player Names**: Personalize the game experience.

**Customizable black and white pieces player**: Two ways to distribute the black and white parties.

**Timed Turns**: Each player has 30s to place piece.

**Undo button**: Each player gets two opportunities to undo their last moves.

**Dynamic Redraw**: Record the action dynamically and can reshow the board based on the history.

**Winning Detection**: Automatically identifies the winner.

**Score Tracking**: Able to track the 2 players' scores.

**Restart Option**: Swap player roles and start a new game.

**How to Play**

**Launch the program.
**Input player names and assign black and white pieces (randomly or manually).
**Take turns clicking on the board to place your pieces.
**Try to connect five pieces in a row to win.
**Use the "Undo" button if you need to revert your move (limited to 2 per player).
**The game will announce the winner once a player meets the win condition.

## Prerequisites

**Environment**: Python version ≥ 3.8

**Tkinter**: Library for creating graphical user interfaces (GUIs). In this project, it is very important for the canvas display.

**Random**: Library for creating the random choice. It would be used when distribute the black and white pieces players.

    import tkinter as tk
    import random
   
## Main

The GomokuGame class based on tk.Tk allows GomokuGame to create and manage the GUI for the game.

    class GomokuGame(tk.Tk):
       def ...

    if __name__ == "__main__":
       game = GomokuGame()
       game.mainloop()
       
### GUI settings

#### Initialize the game setting

     def __init__(self):

        super().__init__()
        self.title("Gomoku Game")
        self.geometry("600x650")
        self.cell_size = 30
        self.board_size = 15
        self.board = [[0] * self.board_size for _ in range(self.board_size)]
        self.current_player = 1
        self.remaining_time = 30

        # Initialize the player information
        self.player_black = None
        self.player_white = None
        self.scores = {}  # Dictionary to store player scores

        # Initialize the distribution mode
        self.distribution_mode = tk.StringVar(value="random")  # Default: random
        self.black_player_choice = tk.StringVar(value="player1")  # Default: Player 1
        self.black_player_choice_frame = None  # Initialize the black choice frame status (showing/hiding)

        # Game history and Undo settings
        self.history = []  # List to store move history [(x, y, player)]
        self.black_undo_remaining = 2  # Black player's undo chances
        self.white_undo_remaining = 2  # White player's undo chances

        # Start with welcome screen
        self.show_welcome_screen()
#### Welcome Page (Page 1)

In Welcome page, "how to play" button and "continue" button was added to the canvas. "how to play" button will go to the Rule Page, while "Continue" button will skip rule page and go to the username input and mode setting page.


    def show_welcome_screen(self):
        '''
        **Function**
        Display the welcome screen for the Gomoku game.

        **Parameters**
        None

        **Returns**
        None
        '''
        self.clear_screen()
        self.canvas = tk.Canvas(self, width=600, height=650, bg="lightblue")
        self.canvas.pack()
        self.canvas.create_text(
            300, 200, text="Welcome to Gomoku Game", font=("Arial", 24, "bold")
        )

        # Create button to go to rules page
        Rules_button = tk.Button(self, text="How to play", font=("Arial", 16), bg="white", command=self.show_rules_screen)
        self.canvas.create_window(300, 350, window=Rules_button)
        
        # Add continue button and set the next page
        self.continue_button(command=self.show_username_input_screen)

Rule Page display setting, including a "Continue" button that can go to the username input and mode seletion page.

    def show_rules_screen(self):
        '''
        **Function**
        Display how to play the game.

        **Parameters**
        None

        **Returns**
        None
        '''
        self.clear_screen()
        self.canvas = tk.Canvas(self, width=600, height=650, bg="lightyellow")
        self.canvas.pack()
        rules = """
        Rules:
        1. Black goes first, followed by White.
        2. Players take turns placing a piece.
        3. Each player has 30 seconds per turn.
        4. The first player to place 5 consecutive pieces 
           horizontally, vertically, or diagonally wins.
        5. Each player has 2 undo chances per game.
        """
        self.canvas.create_text(300, 200, text="How to Play", font=("Arial", 24, "bold"))
        self.canvas.create_text(300, 350, text=rules, font=("Arial", 14), justify="left")
        
        # Add the continue button to the page.
        self.continue_button(command=self.show_username_input_screen)


#### Username input and mode selection page (Page 2)
    def show_username_input_screen(self):
        '''
        **Function**
        Display the username input screen and game settings, 
        including username input and black and white pieces player distrbution selection (Random/Custom)

        **Parameters**
        None

        **Returns**
        None
        '''
        self.clear_screen()
        self.canvas = tk.Canvas(self, width=600, height=650, bg="lightblue")
        self.canvas.pack()

        # Instructions
        self.canvas.create_text(330, 100, text="Enter User Names", font=("Arial", 24, "bold"))

        # Player 1 name entry setting
        tk.Label(self, text="Player 1:", font=("Arial", 16), bg="white").place(x=150, y=150)
        player1_entry = tk.Entry(self, font=("Arial", 16))
        player1_entry.place(x=250, y=150)

        # Player 2 name entry setting
        tk.Label(self, text="Player 2:", font=("Arial", 16)).place(x=150, y=200)
        player2_entry = tk.Entry(self, font=("Arial", 16))
        player2_entry.place(x=250, y=200)

        # Distribution mode selection
        tk.Label(self, text="Distribution Mode:", font=("Arial", 16)).place(x=100, y=250)
        # "Random": use random function to distribute black and white, hide the black player choice frame
        tk.Radiobutton(
            self, text="Random", variable=self.distribution_mode, value="random", font=("Arial", 14),
            command=self.hide_black_player_choice, bg="lightblue"
        ).place(x=290, y=250)
        # "Custom": custom the black and white by players, show the black player choice frame
        tk.Radiobutton(
            self, text="Custom", variable=self.distribution_mode, value="custom", font=("Arial", 14),
            command=self.show_black_player_choice, bg="lightblue"
        ).place(x=400, y=250)

        # Black player choice (only for custom mode)
        self.black_player_choice_frame = tk.Frame(self)
        self.black_player_choice_frame.place(x=100, y=300)
        tk.Label(self.black_player_choice_frame, text="Choose Black Player:", font=("Arial", 16)).pack(side="left")
        tk.Radiobutton(
            self.black_player_choice_frame, text="Player 1", variable=self.black_player_choice, value="player1", font=("Arial", 14)
        ).pack(side="left")
        tk.Radiobutton(
            self.black_player_choice_frame, text="Player 2", variable=self.black_player_choice, value="player2", font=("Arial", 14)
        ).pack(side="left")
        self.hide_black_player_choice()  # Initially hide black choice

        # Create Submit button
        submit_button = tk.Button(
            self, text="Submit", font=("Arial", 16),
            command=lambda: self.assign_players(player1_entry.get(), player2_entry.get())
        )
        submit_button.place(x=250, y=450)

    def hide_black_player_choice(self):
        '''
        **Function**
        Hide the black player choice frame.

        **Parameters**
        None

        **Returns**
        None
        '''
        if self.black_player_choice_frame:
            self.black_player_choice_frame.place_forget()

    def show_black_player_choice(self):
        '''
        **Function**
        Show the black player choice frame.

        **Parameters**
        None

        **Returns**
        None
        '''
        if self.black_player_choice_frame:
            self.black_player_choice_frame.place(x=100, y=300)
#### Player assignment result page (Page 3)
    def assign_players(self, player1, player2):
        '''
        **Function**
        Assign players to black and white pieces based on user input or random distribution.

        **Parameters**
        player1 (str): Name of Player 1.
        player2 (str): Name of Player 2.

        **Returns**
        None
        '''
        if not player1 or not player2:
            return

        if self.distribution_mode.get() == "random":
            if random.choice([True, False]):
                self.player_black, self.player_white = player1, player2
            else:
                self.player_black, self.player_white = player2, player1
       
        elif self.distribution_mode.get() == "custom":
            if self.black_player_choice.get() == "player1":
                self.player_black, self.player_white = player1, player2
            elif self.black_player_choice.get() == "player2":
                self.player_black, self.player_white = player2, player1
            else:
                return

        self.scores.setdefault(self.player_black, 0)
        self.scores.setdefault(self.player_white, 0)

        self.show_player_assignment_result()
    
    # player assignment result display setting (Page 3)
    def show_player_assignment_result(self):
        '''
        **Function**
        Show the result of the player assignment.

        **Parameters**
        None

        **Returns**
        None
        '''
        self.clear_screen()
        self.canvas = tk.Canvas(self, width=600, height=650, bg="lightyellow")
        self.canvas.pack()
        Results = f"Black: {self.player_black}\nWhite: {self.player_white}"
        self.canvas.create_text(300, 200, text="Player Distribution", font=("Arial", 24, "bold"))
        self.canvas.create_text(300, 250, text=Results, font=("Arial", 20))
        self.continue_button(command=self.start_game)


#### Main board display page (Page 4)

    def start_game(self):
        '''
        **Function**
        Start the main game screen, resetting all game states and timers.

        **Parameters**
        None

        **Returns**
        None
        '''
        self.clear_screen()
        self.history = []
        self.black_undo_remaining = 2
        self.white_undo_remaining = 2

        self.undo_button = tk.Button(
            self, text="Undo", font=("Arial", 16), command=self.undo_move, bg="white"
        )
        self.undo_button.pack(pady=10)

        self.timer_label = tk.Label(
            self, text=f"Black's time left: {self.remaining_time}s", font=("Arial", 16)
        )
        self.timer_label.pack()

        self.canvas = tk.Canvas(
            self,
            width=self.board_size * self.cell_size,
            height=self.board_size * self.cell_size,
            bg="burlywood",
        )
        self.canvas.pack()
        self.canvas.bind("<Button-1>", self.place_piece)
        self.draw_board()
        self.update_timer()
        self.update_undo_button()

    def draw_board(self):
        '''
        **Function**
        Draw the game board grid lines.

        **Parameters**
        None

        **Returns**
        None
        '''
        for i in range(self.board_size):
            self.canvas.create_line(
                0, i * self.cell_size, self.board_size * self.cell_size, i * self.cell_size
            )
            self.canvas.create_line(
                i * self.cell_size, 0, i * self.cell_size, self.board_size * self.cell_size
            )

    def place_piece(self, event):
        '''
        **Function**
        Handle piece placement on the board when a player clicks.
        Record the event in history dictionary.

        **Parameters**
        event (tkinter.Event): The mouse click event.

        **Returns**
        None
        '''
        x, y = event.x // self.cell_size, event.y // self.cell_size
        if x >= self.board_size or y >= self.board_size or self.board[y][x] != 0:
            return

        self.board[y][x] = self.current_player
        color = "black" if self.current_player == 1 else "white"
        self.canvas.create_oval(
            x * self.cell_size + 5,
            y * self.cell_size + 5,
            (x + 1) * self.cell_size - 5,
            (y + 1) * self.cell_size - 5,
            fill=color,
        )

        # Record the history of player actions
        self.history.append((x, y, self.current_player))

        # Check if there are five pieces in a row every time
        if self.check_winner(x, y):
            winner = self.player_black if self.current_player == 1 else self.player_white
            self.display_winner(f"Game Over! {winner} wins!")
            return

        # Swap the player
        self.current_player = 3 - self.current_player
        
        # Set the remaining time per turn
        self.remaining_time = 30
        
        self.update_timer_label()
        self.update_undo_button()

    # Regret/Undo button
    def undo_move(self):
        '''
        **Function**
        Undo the last two moves made by both players.

        **Parameters**
        None

        **Returns**
        None
        '''
        # if there are not 2 steps in history, undo button cannot work
        if len(self.history) < 2:
           return

        # Check the chance for current player
        if self.current_player == 1 and self.black_undo_remaining > 0:
            self.black_undo_remaining -= 1
        elif self.current_player == 2 and self.white_undo_remaining > 0:
            self.white_undo_remaining -= 1
        else:
            return

        # Undo two steps
        for _ in range(2):
            if not self.history:
               break
            x, y, player = self.history.pop()
            self.board[y][x] = 0  # clear the history for these two steps

        # Redraw the board according to the history
        self.redraw_board() 
        self.update_undo_button()

        # Update the timer
        self.remaining_time = 30
        self.update_timer_label()


    def update_undo_button(self):
        '''
        **Function**
        Update the undo button text and enable/disable status based on remaining chances.

        **Parameters**
        None

        **Returns**
        None
        '''
        if self.current_player == 1:
            self.undo_button.config(text=f"Undo ({self.black_undo_remaining})")
        else:
            self.undo_button.config(text=f"Undo ({self.white_undo_remaining})")

        if (self.current_player == 1 and self.black_undo_remaining == 0) or \
           (self.current_player == 2 and self.white_undo_remaining == 0):
            self.undo_button.config(state=tk.DISABLED)
        else:
            self.undo_button.config(state=tk.NORMAL)

    def redraw_board(self):
        '''
        **Function**
        Redraw the entire board based on the current game state.

        **Parameters**
        None

        **Returns**
        None
        '''
        self.canvas.delete("all")
        self.draw_board()
        for x, y, player in self.history:
            color = "black" if player == 1 else "white"
            self.canvas.create_oval(
                x * self.cell_size + 5,
                y * self.cell_size + 5,
                (x + 1) * self.cell_size - 5,
                (y + 1) * self.cell_size - 5,
                fill=color,
            )

    def update_timer(self):
        '''
        **Function**
        Decrease the timer and check if a player has run out of time.

        **Parameters**
        None

        **Returns**
        None
        '''
        if self.remaining_time > 0:
            self.remaining_time -= 1
            self.update_timer_label()
            self.after(1000, self.update_timer)
        else:
            winner = self.player_white if self.current_player == 1 else self.player_black
            self.display_winner(f"{winner} wins by timeout!")

    def update_timer_label(self):
        '''
        **Function**
        Update the timer label to show the current player's remaining time.

        **Parameters**
        None

        **Returns**
        None
        '''
        player = self.player_black if self.current_player == 1 else self.player_white
        self.timer_label.config(text=f"{player}'s time left: {self.remaining_time}s")

Winner detection logic

    def count_in_one_direction(self, x, y, dx, dy):
        '''
        **Function**
        Count consecutive pieces of the current player in a given direction.

        **Parameters**
        x (int): The starting x-coordinate.
        y (int): The starting y-coordinate.
        dx (int): The step in the x-direction.
        dy (int): The step in the y-direction.

        **Returns**
        int: The count of consecutive pieces in the given direction.
        '''
        count = 0
        while (
            0 <= x < self.board_size
            and 0 <= y < self.board_size
            and self.board[y][x] == self.current_player
        ):
            count += 1
            x += dx
            y += dy
        return count

    def check_winner(self, x, y):
        '''
        **Function**
        Check if the current player has achieved 5 pieces in a row.

        **Parameters**
        x (int): The x-coordinate of the last placed piece.
        y (int): The y-coordinate of the last placed piece.

        **Returns**
        bool: True if the current player has won, False otherwise.
        '''
        directions = [(1, -1), (1, 1), (1, 0), (0, 1)]
        for dx, dy in directions:
            count = 1
            count += self.count_in_one_direction(x + dx, y + dy, dx, dy)
            count += self.count_in_one_direction(x - dx, y - dy, -dx, -dy)
            if count >= 5:
                return True
        return False
#### Gameover page and Ending page settings

    def display_winner(self, message):
        '''
        **Function**
        Display the winner and redraw the board for the game end screen.

        **Parameters**
        message (str): The message to display (e.g., "Game Over! Player X wins!")

        **Returns**
        None
        '''
        self.canvas.unbind("<Button-1>")
        self.clear_screen()
        winner = self.player_black if self.current_player == 1 else self.player_white
        self.scores[winner] += 1

        winner_label = tk.Label(self, text=message, font=("Arial", 32, "bold"), fg="red")
        winner_label.pack(pady=50)

        self.canvas = tk.Canvas(self,width=self.board_size * self.cell_size,height=self.board_size * self.cell_size,bg="burlywood"
        )
        self.canvas.pack()
        self.redraw_board() 

        self.continue_button(command=self.Ending_page, x=225,y=-20)

        
    # Ending page (Page 6)
    def Ending_page(self):
        '''
        **Function**
        Display the ending page with the current scores and options to restart or quit.

        **Parameters**
        None

        **Returns**
        None
        '''
        self.canvas.unbind("<Button-1>")
        self.clear_screen()
        self.canvas = tk.Canvas(self, width=600, height=650, bg="lightyellow")
        self.canvas.pack()

        score_text = f"""
        Scores:
        {self.player_black}: {self.scores[self.player_black]}
        {self.player_white}: {self.scores[self.player_white]}
        """
        score_label = tk.Label(self, text=score_text, font=("Arial", 24,"bold"), bg="white")
        score_label.pack(pady=10)

        restart_button = tk.Button(
            self, text="Restart", font=("Arial", 16), command=self.restart_game
        )
        restart_button.pack(pady=10)

        quit_button = tk.Button(
            self, text="Quit", font=("Arial", 16), command=self.quit
        )
        quit_button.pack(pady=10)
    

    def restart_game(self):
        '''
        **Function**
        Restart the game with swapped player roles (black/white).

        **Parameters**
        None

        **Returns**
        None
        '''
        self.player_black, self.player_white = self.player_white, self.player_black
        self.board = [[0] * self.board_size for _ in range(self.board_size)]
        self.current_player = 1
        self.remaining_time = 30
        self.start_game()
#### Supportive function

def clear_screen()
def continue_button()


