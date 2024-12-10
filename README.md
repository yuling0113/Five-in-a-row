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

## How to play

Launch the program.

Input player names and assign black and white pieces (randomly or manually).

Take turns clicking on the board to place your pieces.

Try to connect five pieces in a row to win.

Use the "Undo" button if you need to revert your move (limited to 2 per player).

The game will announce the winner once a player meets the win condition.


## Prerequisites

**Environment**: Python version ≥ 3.8

**Tkinter**: Library for creating graphical user interfaces (GUIs). In this project, it is very important for the canvas display.

**Random**: Library for creating the random choice. It would be used when distribute the black and white pieces players.

    import tkinter as tk
    import random
   
## Project Structure

gomoku/


├── src/                     # Source code


│   ├── main.py              # Main game file containing the gameplay logic


│   ├── GUI settings(): Initialized the game settings, including welcome


│   │                   page, rule page, mode selection and player assignment


│   ├── game screen():       # Initialized the game settings


│   │    ├── start_game(): Initializes the game screen, resets states, and sets up the


│   │    │                 board.


│   │    ├── clear_screen(): load a new game interface.


│   │    ├── draw_board(): Draws the grid lines of the board.


│   │    ├── place_piece(): Handles piece placement and updates the game state.


│   │    ├── undo_move(): Implements the undo functionality for both players.


│   │    ├── redraw_board(): Redraws the board based on the current game state.


│   │    ├── update_timer(): Decreases the timer and checks for timeouts.


│   │    ├── update_timer_label(): Updates the GUI label to display the current player's


│   │    │                         remaining time.


│   │    └── update_undo_button(): Updates the text and status of the undo button based


│   │                               on remaining chances.


│   └── Game Over and Winner Display():


│       ├── display_winner(): Displays the winner when the game ends.


│       ├── Ending Page (): Displays the final scores of the players. Provides options


│                           to restart the game or quit.


├── README.md                # Documentation for the project


└──  requirements.txt        # Dependencies required for the project



## Main

The GomokuGame class based on Tkinter allows GomokuGame to create and manage the GUI for the game.

    class GomokuGame(tk.Tk):
       def ...

    if __name__ == "__main__":
       game = GomokuGame()
       game.mainloop()
       
### GUI settings

#### Initialize the game setting

     def __init__(self):

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
       

    def show_rules_screen(self):
        '''
        **Function**
        Display how to play the game.

        **Parameters**
        None

        **Returns**
        None
        '''

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

    def draw_board(self):
        '''
        **Function**
        Draw the game board grid lines.

        **Parameters**
        None

        **Returns**
        None
        '''
        
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
        
        # Record the history of player actions

        # Check if there are five pieces in a row every time

        # Swap the player
        
        # Set the remaining time per turn

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

    def redraw_board(self):
        '''
        **Function**
        Redraw the entire board based on the current game state.

        **Parameters**
        None

        **Returns**
        None
        '''

    def update_timer(self):
        '''
        **Function**
        Decrease the timer and check if a player has run out of time.

        **Parameters**
        None

        **Returns**
        None
        '''

    def update_timer_label(self):
        '''
        **Function**
        Update the timer label to show the current player's remaining time.

        **Parameters**
        None

        **Returns**
        None
        '''

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

    def restart_game(self):
        '''
        **Function**
        Restart the game with swapped player roles (black/white).

        **Parameters**
        None

        **Returns**
        None
        '''
       
#### Supportive function

def clear_screen()
def continue_button()

## Future improvements

Add an AI opponent for single-player mode.
Implement saving and loading game states.
Enhance the graphical interface with more visual effects.
Include more advanced move analysis and hints for players.

## License

This project is free to use and modify


