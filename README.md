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
