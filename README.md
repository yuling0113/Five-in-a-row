# MineGomoku Game
An upgraded Gomoku (Five in a Row) game with mines in the board based on Python and Tkinter commands is displayed with special function including customizable game setting before starting the game, undo option and draw option while playing, and score recording, restart option, and quit option while gameover. This Mine-Gomoku game provides tradition mode of gomoku (number of mines = 0) and orginal mode with cells containing mines where players cannot place pieces and cannot know whether the cell contains mine until it is clicked, which increases the fun of the game.

## Features

**Rule Reading Selection**: Players can choose whether skip the rule reading.

**Customizable Game Setting**: Personalize the game experience with customizable usernames, provide two ways (Random/Custom) to distribute the black and white parties and provide the customizable number of mines.

**Timed Turns**: Each player has 30s to place piece.

**Undo button**: Each player gets two opportunities to undo their last moves.

**Draw button**: Players can draw the game immediately without winners.

**Redraw**: Record the action dynamically and can reshow the board based on the history.

**Redisplay**: The history and all the mines would redisplay when the game is over. 

**Winning Detection**: Automatically identifies the winner if five consecutive pieces in a row are detected.

**Score Tracking**: Able to track the 2 players' scores.

**Restart Option**: Swap player roles and start a new game.

## How to play

Run the "Final Version_MineGomokuGame" to launch the program.

Input player names, assign black and white pieces (randomly or manually), and set the number of mines.

Take turns clicking on the board to place your pieces.

Try to connect five pieces in a row to win.

Use the "Undo" button (Max: 2 times) if you need to revert your move (limited to 2 per player).

Use the "Draw" button to draw the game without scoring.

The game will announce the winner once a player meets the win condition.

Use the "Restart" button to swap players and restart the game.

## Game Logic

**Winner Checking**:
Check as long as the player place the piece.
check_winner(): Verifies if a player has achieved five consecutive pieces in four directions (1,0) (-1,0) (1,-1) (-1,1).
-- count_in_one_direction(): Counts consecutive pieces of the current player in a specified direction.

**Mines distribution**:
Limit the range of the number of mines in GUI setting and raise error when the input number is outside the range[0,150]. In other functions, use "random" command to distribute the mines and make sure at the beginning of the game, there are at least five consecutive empty spaces.  

Starting from (0, 0), the set_disabled_cells() function randomly assigns mines and validates the distribution using five_consecutive_empty_spaces_exist(), which checks if at least five consecutive empty spaces exist on the board in any of the four directions. This is achieved by iterating through each cell and using count_consecutive_empty() to count consecutive empty spaces in a given direction. If the conditions are not met, the mine distribution is regenerated.

set_disabled_cells(): Randomly places mines while ensuring valid gameplay.
-- five_consecutive_empty_spaces_exist(): Checks if there are at least five consecutive empty spaces.
-- count_consecutive_empty(): Verifies the number of consecutive empty cells in a direction.

**Undo button**：When undo_move() is triggered, it checks if at least two moves exist in the history and deducts one undo chance from the current player. The last two moves are removed, the board is updated, and redraw_board() is called to reflect the changes. The timer resets to 30 seconds for the current player, and the undo button updates to reflect the remaining chances or becomes disabled if no chances remain. As players should pay "Undo", the mines would disappear in the screen after clicking "Undo".


## Prerequisites

**Environment**: Python version ≥ 3.8

**Tkinter**: Library for creating graphical user interfaces (GUIs). In this project, it is very important for the canvas display.

**tkinter.messagebox**: Library for showing error messages while using tkinter to realize GUIs.

**Random**: Library for creating the random choice. It would be used when distribute the black and white pieces players.

    import tkinter as tk
    import tkinter.messagebox as messagebox
    import random
   
## Project Structure

Minegomoku/
>src/                     # Source code
>
>> Pre-requirements
>> main.py                # Main game file containing the gameplay logic
>> 
>>>GUI settings(): Initialized the game settings, including welcome page, rule page, game setting page, winner/draw page, and ending page
>>>>
>>>>show_welcome_screen(): Displays the welcome page with a "Continue" button.
>>>>
>>>>show_rules_screen(): Displays the game rules.
>>>>
>>>>show_username_input_screen(): Displays settings for username input, player assignment, and mine customization.
>>>>
>>>>show_player_assignment_result(): Displays the assigned player roles (Black/White).
>>>>
>>>> Ending_page(): Displays the ending page with final scores and options to restart or quit.
>>>>
>>>> Others: continue_button()&clear_screen()
>>>
>>>Game Screen:
>>>
>>>>start_game(): Initializes the game screen, resets states, and sets up the board.
>>>>
>>>>draw_board(): Draws the grid lines of the board.
>>>>
>>>>place_piece(): Handles piece placement, record the current steps and updates the game state.
>>>>
>>>>undo_botton: deletes the last 2 steps and offers opportunity to replace the pieces. Include undo_move()&update_undo_button()
>>>>
>>>>redraw_board(): Redraws the board based on the current game state.
>>>>
>>>>update timer: Display the decrease in time for current player. update_timer(): Decreases the timer and checks for timeouts & update_timer_label(): display the current player's name
>>>>
>>>>draw_button: Draw the game quickly without winners and display a gameover page and reproduce the board. draw()
>>>>
>>>Game Over and Winner Display():
>>>
>>>>display_winner(): Displays the winner, the history of steps, and all the mines when the game ends
>>>>
>>>>draw():Displays Display "Draw", the history of steps, and all the mines when the game ends
>>>>
>>>>Ending Page (): Displays the final scores of the players. Provides options to restart the game or quit.
>>>>
>README.md                # Documentation for the project
>
>

## Main

The MineGomokuGame class based on Tkinter allows GomokuGame to create and manage the GUI for the game.

    class MineGomokuGame(tk.Tk):
       def ...

    if __name__ == "__main__":
       game = MineGomokuGame()
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

        # Initialze the mine setting (Disabled cells setup)
        self.disabled_cells = set()  # Store coordinates of disabled cells
        self.num_disabled_cells = 0  # Number of disabled cells

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
        including username input, black and white pieces player distrbution selection (Random/Custom), and number of mines setting

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
    def draw_button(self):
        '''
        **Function**
        Draw the game and go to the ending page.

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
       
#### Gameover page， Draw page and Ending page settings

    def display_winner(self, message):
        '''
        **Function**
        Display the winner and redraw the board for the game end screen.

        **Parameters**
        message (str): The message to display (e.g., "Game Over! Player X wins!")

        **Returns**
        None
        '''
        
    def draw_button(self):
        '''
        **Function**
        Draw the game and go to the ending page.

        **Parameters**
        None

        **Returns**
        None
        '''
    
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
……

## Future improvements

Add an AI opponent for single-player mode.
Implement saving and loading game states.
Enhance the graphical interface with more visual effects.
Include more advanced move analysis and hints for players.

## License

This project is free to use and modify


