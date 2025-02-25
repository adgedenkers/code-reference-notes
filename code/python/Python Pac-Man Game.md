# Build a Python Pac-Man Game with PyGame (Step-by-Step)

In this tutorial, we will build a Pac-Man game using Python and Pygame. This project is perfect for beginners who want to learn about game development, handling user input, and working with grids in a 2D environment.

By the end of this tutorial, you'll have a functional, yet classic arcade game where Pac-Man moves around a maze, eats pellets, and avoids ghosts.

This project covers essential programming concepts, including:

- Using **Pygame** to create a simple 2D game
- Handling **keyboard inputs** to control movement
- Implementing **collision detection** for ghosts and pellets
- Working with **grids** to design a game level

Let’s get started!

## Step 1: Setting Up the Project

Before we start coding, let’s set up our [Python project](https://hackr.io/blog/python-projects):

1. Make sure Python is installed on your computer. If not, download it from the [official Python website](https://www.python.org/).  
2. Open your favorite code editor or IDE.  
3. Create a new Python file, for example, `pacman.py`.

Before we start coding, **Install Pygame** if you haven't already. You can install it using:

```python
pip install pygame
```

Great, now, let's dive head first into our [Python editor](https://app.hackr.io/editors/python) to get this build started.

## Step 2: Understanding How the Game Works

The game consists of a **maze**, **Pac-Man**, **pellets**, and **ghosts**. The goal is to move Pac-Man around the maze, collect all the pellets, and avoid the ghosts.

- Walls block movement.
- Pellets increase the score when collected.
- Ghosts move randomly and will end the game if they catch Pac-Man.
- The player has **3 lives** before the game is over.

We’ll use Pygame to create our interface, and when we're done, we should have something that looks like this:

![GUI for Pac-Man game built with Pygame in Python.](https://i.imgur.com/gsKMa8N.gif)

## Step 3: Importing Required Modules

To start, we need to import Pygame and other necessary modules:

```python
import pygame
import random
```

Why Do We Use These Modules?

- **pygame**: Provides the game framework for handling graphics, input, and events.
- **random**: We use [Python random](https://hackr.io/blog/python-random-function) to help with random ghost movement.

## Step 4: Designing the Game Layout

We will now implement the game layout using Pygame to create the game window and draw the maze.

```python
# Initialize pygame
pygame.init()

# Constants
WIDTH, HEIGHT = 600, 600  # The dimensions of the game window
GRID_SIZE = 30  # Each cell in the grid is 30x30 pixels
ROWS, COLS = HEIGHT // GRID_SIZE, WIDTH // GRID_SIZE  # Number of rows and columns in the grid
WHITE = (255, 255, 255)  # Color for empty paths and pellets
BLACK = (0, 0, 0)  # Background color
YELLOW = (255, 255, 0)  # Color for Pac-Man
RED = (255, 0, 0)  # Color for ghosts
BLUE = (0, 0, 255)  # Color for walls

# Maze grid (1 = Wall, 0 = Empty path, 2 = Pellet, 3 = Ghost, 4 = Pac-Man)
maze = [
    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
    [1, 4, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 0, 1],
    [1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 1],
    [1, 0, 1, 3, 1, 0, 1, 3, 0, 0, 0, 0, 1, 3, 1, 0, 1, 3, 0, 1],
    [1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 0, 1],
    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
]

# Store pellet positions separately
pellet_positions = [(row_idx, col_idx) for row_idx, row in enumerate(maze) for col_idx, cell in enumerate(row) if cell == 2]
```

**Breakdown:**

- We **initialize Pygame** and set up the game **window size (600x600 pixels)**.
- The **constants** define colors, grid size, and window dimensions.
- The game grid is represented as a **list of lists**, where:
    - `1` represents **walls** (blue blocks) that block movement.
    - `0` represents **empty paths** where Pac-Man and ghosts can move.
    - `2` represents **pellets** (small dots Pac-Man collects for points).
    - `3` represents **ghosts**, which move randomly and chase Pac-Man.
    - `4` represents **Pac-Man**, the player-controlled character.
- The [list comprehension](https://hackr.io/blog/python-list-comprehensions) extracts all pellet positions (`2`) from the maze into `pellet_positions`. This allows us to:
    - **Track pellet positions independently** of the grid, making it easier to update the display when ghosts move over them.
    - **Ensure accuracy** when Pac-Man eats a pellet, so we can properly update the maze and the score.
    - **Restore pellets when ghosts move away**, ensuring that eaten pellets don't disappear permanently when ghosts walk over them.

This structure allows us to **easily render the maze** and handle game logic such as movement, collisions, and scoring!

## Step 5: Drawing the Maze

We will now [define a function](https://hackr.io/blog/python-define-function) to **render the game maze** using Pygame. This function iterates through the `maze` grid and draws the appropriate elements on the screen.

```python
def draw_maze(screen):
    for row_idx, row in enumerate(maze):
        for col_idx, cell in enumerate(row):
            x, y = col_idx * GRID_SIZE, row_idx * GRID_SIZE
            if cell == 1:
                pygame.draw.rect(screen, BLUE, (x, y, GRID_SIZE, GRID_SIZE))
            elif cell == 2:
                pygame.draw.circle(screen, WHITE, (x + GRID_SIZE // 2, y + GRID_SIZE // 2), 5)
            elif cell == 3:
                pygame.draw.circle(screen, RED, (x + GRID_SIZE // 2, y + GRID_SIZE // 2), 12)
            elif cell == 4:
                pygame.draw.circle(screen, YELLOW, (x + GRID_SIZE // 2, y + GRID_SIZE // 2), 12)
```

**Breakdown:**

- **Looping through the maze**:
    
    - We use [Python enumerate](https://hackr.io/blog/python-enumerate-function) to iterate over the `maze`, extracting the **row index (`row_idx`)** and the **row itself (`row`)**.
    - Another `enumerate()` loop extracts each **column index (`col_idx`)** and its respective **cell value (`cell`)**.
- **Determining screen position**:
    
    - `x, y = col_idx * GRID_SIZE, row_idx * GRID_SIZE` converts grid indices into pixel coordinates so that each game element is drawn at the correct location.
- **Rendering different elements**:
    
    - If `cell == 1`: Draws a **wall** as a blue rectangle (`pygame.draw.rect`).
    - If `cell == 2`: Draws a **pellet** as a small white circle at the center of the grid cell (`pygame.draw.circle`).
    - If `cell == 3`: Draws a **ghost** as a red circle with a radius of 12 pixels.
    - If `cell == 4`: Draws **Pac-Man** as a yellow circle at the center of the grid cell.

**Why This Matters:**

- This function ensures that the **maze is properly displayed** each frame.
- By **looping through the grid**, we dynamically update the screen based on the current game state.
- Ensuring that each element is drawn **at the correct position** keeps the game looking consistent and working smoothly.

This function will be called in the **main game loop** to continuously refresh the display as Pac-Man and the ghosts move around!

## Step 6: Retrieving Pac-Man and Ghost Positions

In this step, we define a function to locate **Pac-Man** and the **ghosts** within the maze. This function scans the grid and returns their positions, which will be used for movement and collision detection.

```python
# Get Pac-Man & Ghost positions
def get_positions():
    pacman_pos = None
    ghost_positions = []
    for row_idx, row in enumerate(maze):
        for col_idx, cell in enumerate(row):
            if cell == 4:
                pacman_pos = [row_idx, col_idx]
            elif cell == 3:
                ghost_positions.append([row_idx, col_idx])
    return pacman_pos, ghost_positions

pacman_pos, ghost_positions = get_positions()
score = 0
lives = 3
```

**Breakdown:**

- **Initializing positions**:
    
    - `pacman_pos = None`: Placeholder for Pac-Man’s position.
    - `ghost_positions = []`: Empty list to store multiple ghost positions.
- **Looping through the maze**:
    
    - We use `enumerate()` to iterate over **each row (`row_idx`)** and **each cell (`cell`)**.
    - If `cell == 4`: Store Pac-Man’s position as `[row_idx, col_idx]`.
    - If `cell == 3`: Append the ghost’s position to `ghost_positions`.
- **Returning positions**:
    
    - This function returns `pacman_pos` and `ghost_positions` so that the game can track and update their movements.

**Why This Matters:**

- **Efficient Tracking**: This function allows us to dynamically **retrieve and update** positions without hardcoding them.
- **Collision Detection**: Knowing where Pac-Man and the ghosts are is essential for detecting when Pac-Man gets caught.
- **Gameplay Mechanics**: The game logic depends on these positions to ensure Pac-Man and ghosts move correctly.

By running `get_positions()`, we retrieve the initial locations of Pac-Man and ghosts and set up the **score (starting at 0)** and **lives (starting at 3)**, providing the foundation for game progression.

## Step 7: Moving Pac-Man

In this step, we implement the function to **control Pac-Man's movement** based on player input. The function updates Pac-Man's position while ensuring valid movement within the maze.

```python
def move_pacman(direction, screen):
    global pacman_pos, score
    row, col = pacman_pos
    new_row, new_col = row, col
    
    if direction == "UP":
        new_row -= 1
    elif direction == "DOWN":
        new_row += 1
    elif direction == "LEFT":
        new_col -= 1
    elif direction == "RIGHT":
        new_col += 1
    
    if maze[new_row][new_col] != 1:  # Check if the new position is not a wall
        if (new_row, new_col) in pellet_positions:  # Check if Pac-Man eats a pellet
            pellet_positions.remove((new_row, new_col))  # Remove the pellet
            score += 10  # Increase the score
        
        maze[row][col] = 0  # Clear old position
        maze[new_row][new_col] = 4  # Move Pac-Man
        pacman_pos = [new_row, new_col]  # Update Pac-Man’s position
    
    check_collision(screen)  # Check if Pac-Man collides with a ghost
```

**Breakdown:**

- **Reading Pac-Man’s current position**:
    
    - The function retrieves Pac-Man’s row (`row`) and column (`col`) from `pacman_pos`.
    - `new_row` and `new_col` store the potential new position.
- **Updating Pac-Man’s position**:
    
    - Based on `direction`, we update `new_row` or `new_col`.
    - We ensure Pac-Man **does not move into a wall (`maze[new_row][new_col] != 1`)**.
- **Handling pellets**:
    
    - If Pac-Man moves onto a pellet (`2` in the grid), we remove it from `pellet_positions` and **increase the score by 10**.
- **Updating the grid**:
    
    - The old position (`row, col`) is cleared (`0` in the maze).
    - The new position (`new_row, new_col`) is updated to `4` (Pac-Man).
- **Checking for collisions**:
    
    - We call `check_collision(screen)` to detect if Pac-Man has collided with a ghost.

**Why This Matters:**

- **Enforces movement rules**: Prevents Pac-Man from moving through walls.
- **Tracks score dynamically**: Ensures pellets are removed and score is updated.
- **Enables real-time interaction**: Pac-Man moves fluidly in response to user input.
- **Maintains game state**: Ensures the grid updates accurately, reflecting Pac-Man’s movement.

This function will be called whenever the player presses an arrow key, allowing Pac-Man to navigate the maze and interact with the game environment!

## Step 8: Moving the Ghosts

In this step, we implement the function that **moves the ghosts** within the maze. Ghosts move randomly, but they cannot pass through walls. Their movement is handled dynamically to ensure smooth gameplay.

```python
def move_ghosts(screen):
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]  # Right, Left, Down, Up
    for ghost in ghost_positions:
        row, col = ghost
        random.shuffle(directions)  # Randomize movement direction
        
        for dr, dc in directions:
            new_row, new_col = row + dr, col + dc
            
            if maze[new_row][new_col] in [0, 2]:  # Check if new position is an empty path or pellet
                maze[row][col] = 0  # Restore empty path
                if (row, col) in pellet_positions:
                    maze[row][col] = 2  # Restore pellet if ghost was on one
                
                ghost[0], ghost[1] = new_row, new_col  # Update ghost position
                maze[new_row][new_col] = 3  # Move ghost
                break
    check_collision(screen)  # Check if a ghost has collided with Pac-Man
```

**Breakdown:**

- **Ghosts move randomly**:
    
    - The possible directions (`Right, Left, Down, Up`) are shuffled to ensure varied movement.
    - The loop iterates through the shuffled directions, attempting to move the ghost.
- **Valid movement conditions**:
    
    - Ghosts can only move onto empty paths (`0`) or pellets (`2`).
    - If a ghost moves away from a tile that previously had a pellet, the pellet is **restored** to maintain the correct game state.
- **Updating ghost position**:
    
    - The ghost’s previous position is cleared (`0`).
    - If necessary, a pellet is restored (`2`).
    - The ghost’s new position is updated in the maze grid (`3`).
- **Collision detection**:
    
    - After each ghost moves, we check if it has collided with Pac-Man using `check_collision(screen)`, which will handle reducing lives or ending the game.

**Why This Matters:**

- **Ensures dynamic movement**: Ghosts do not follow a predictable path, making the game more challenging.
- **Prevents movement through walls**: The function checks for valid spaces before allowing movement.
- **Maintains pellet visibility**: If a ghost moves off a pellet, the pellet is restored to avoid disappearing permanently.
- **Enhances difficulty**: With random movement, Pac-Man must avoid ghosts without knowing exactly where they will go next.

This function will be called **every game update cycle** to keep the ghosts moving, adding to the challenge of the game!

## Step 9: Checking for Collisions

In this step, we define a function that **detects collisions between Pac-Man and ghosts**. If Pac-Man collides with a ghost, he loses a life. If all lives are lost, the game ends.

```python
def check_collision(screen):
    global lives, pacman_pos
    for ghost in ghost_positions:
        if pacman_pos == ghost:  # Check if Pac-Man and a ghost occupy the same position
            lives -= 1  # Decrease lives count
            
            if lives > 0:
                display_message(screen, "You lost a life!", RED)  # Show a message if lives remain
            if lives == 0:
                display_message(screen, "Game Over!", RED)  # Show game over message
                pygame.quit()
                exit()
            
            # Reset Pac-Man position and update the maze immediately
            old_row, old_col = pacman_pos
            maze[old_row][old_col] = 0  # Clear old position
            pacman_pos = [1, 1]  # Reset Pac-Man to the starting position
            maze[1][1] = 4  # Place Pac-Man back on the grid
```

**Breakdown:**

- **Loop through all ghosts**:
    - The function iterates through `ghost_positions` to check if any ghost shares Pac-Man’s position.
- **Handling a collision**:
    - If Pac-Man and a ghost are at the same position, **Pac-Man loses a life (`lives -= 1`)**.
    - If Pac-Man has lives left, a **message is displayed** to notify the player.
    - If Pac-Man has no lives remaining, the game ends with a **"Game Over!"** message, and Pygame quits.
- **Resetting Pac-Man's position**:
    - The old position in the `maze` grid is cleared (`0`).
    - Pac-Man is **reset to the starting position (`[1,1]`)**, ensuring he respawns safely.
    - The maze grid is updated to **place Pac-Man back at his starting position (`4`)**.

**Why This Matters:**

- **Implements game mechanics**: Losing lives and handling game-over scenarios are crucial for gameplay.
- **Ensures game state consistency**: The function updates the game grid after a collision.
- **Provides player feedback**: Messages notify the player when they lose a life or the game ends.
- **Maintains challenge**: Players must avoid ghosts to keep playing.

This function is **called every game update cycle** to ensure that collisions are detected in real-time!

## Step 10: Displaying Messages to the Player

In this step, we define a function to **display messages on the screen**. This is useful for notifying the player about events such as losing a life or game over and is more user-friendly than using [Python print](https://hackr.io/blog/python-print-function) to show messages in the terminal.

```python
def display_message(screen, message, color=WHITE):
    font = pygame.font.Font(None, 50)  # Set the font size to 50
    text = font.render(message, True, color)  # Render the message text
    text_rect = text.get_rect(center=(WIDTH // 2, HEIGHT // 2))  # Center the text on the screen
    
    screen.blit(text, text_rect)  # Draw the message on the screen
    pygame.display.flip()  # Update the display to show the message
    pygame.time.delay(2000)  # Pause for 2 seconds before continuing
```

**Breakdown:**

- **Creating the font**:
    - `pygame.font.Font(None, 50)`: Loads the default font with a size of `50`.
- **Rendering the text**:
    - `text = font.render(message, True, color)`: Converts the message string into a **Pygame surface** with the given color.
- **Centering the message**:
    - `text_rect = text.get_rect(center=(WIDTH // 2, HEIGHT // 2))`: Ensures the message appears at the center of the screen.
- **Displaying the message**:
    - `screen.blit(text, text_rect)`: Draws the rendered text onto the game screen.
    - `pygame.display.flip()`: Updates the display so the message is shown immediately.
- **Adding a delay**:
    - `pygame.time.delay(2000)`: Pauses the game for **2 seconds**, allowing the player to read the message before gameplay resumes.

**Why This Matters:**

- **Provides player feedback**: Displays messages for important game events (e.g., "You lost a life!", "Game Over!").
- **Enhances user experience**: Gives the player a moment to process the event before continuing.
- **Maintains game flow**: Prevents immediate action after an event, ensuring smoother gameplay transitions.

This function is called whenever an event like losing a life or game over occurs, ensuring clear communication with the player!

## Step 11: The Main Game Loop

The `main()` function runs the core logic of the game, managing **game updates, rendering, and user input** in a continuous [while loop](https://hackr.io/blog/python-while-loop).

```python
def main():
    screen = pygame.display.set_mode((WIDTH, HEIGHT))  # Create the game window
    pygame.display.set_caption("Pac-Man (Beginner Version)")  # Set window title
    clock = pygame.time.Clock()  # Initialize game clock
    
    while True:
        screen.fill(BLACK)  # Clear screen to black before drawing
        draw_maze(screen)  # Render the maze and all game elements
        
        # Display score and lives
        font = pygame.font.Font(None, 36)
        score_text = font.render(f"Score: {score}", True, WHITE)
        lives_text = font.render(f"Lives: {lives}", True, WHITE)
        screen.blit(score_text, (10, 10))
        screen.blit(lives_text, (WIDTH - 100, 10))
        
        move_ghosts(screen)  # Update ghost positions
        pygame.display.flip()  # Update the display with new frame
        
        # Handle user inputs
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                exit()
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_UP:
                    move_pacman("UP", screen)
                elif event.key == pygame.K_DOWN:
                    move_pacman("DOWN", screen)
                elif event.key == pygame.K_LEFT:
                    move_pacman("LEFT", screen)
                elif event.key == pygame.K_RIGHT:
                    move_pacman("RIGHT", screen)
        
        clock.tick(5)  # Control game speed
    
if __name__ == "__main__":
    main()
```

**Breakdown:**

- **Creating the Game Window**:
    
    - `pygame.display.set_mode((WIDTH, HEIGHT))`: Initializes the game window.
    - `pygame.display.set_caption("Pac-Man (Beginner Version)")`: Sets the window title.
    - `pygame.time.Clock()`: Creates a clock to control the game's speed.
- **The Game Loop**:
    
    - `while True`: The game runs continuously until the player quits.
    - `screen.fill(BLACK)`: Clears the screen before redrawing elements.
    - `draw_maze(screen)`: Renders the maze, Pac-Man, ghosts, and pellets.
    - Displays the **score and lives** using `pygame.font.Font`.
    - Calls `move_ghosts(screen)` to move ghosts randomly.
    - Updates the display with `pygame.display.flip()`.
- **Handling User Input**:
    
    - Detects **QUIT event** (`pygame.QUIT`) to close the game.
    - Listens for **arrow key presses** (`pygame.KEYDOWN`) to move Pac-Man.
- **Controlling Game Speed**:
    
    - `clock.tick(5)`: Limits the game to **5 frames per second**, ensuring smooth and manageable gameplay.

**Why This Matters:**

- **Manages the game state**: Ensures all elements update properly.
- **Handles user interaction**: Processes keyboard inputs to move Pac-Man.
- **Regulates game speed**: Prevents the game from running too fast.
- **Renders updates dynamically**: Keeps the game visually responsive.

This function serves as the **heartbeat** of the game, ensuring that everything from movement to display updates works seamlessly!

## Final Code: Pac-Man Game

```python
import pygame
import random

# Initialize pygame
pygame.init()

# Constants
WIDTH, HEIGHT = 600, 600
GRID_SIZE = 30  # Each cell in the grid is 30x30 pixels
ROWS, COLS = HEIGHT // GRID_SIZE, WIDTH // GRID_SIZE
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
YELLOW = (255, 255, 0)
RED = (255, 0, 0)
BLUE = (0, 0, 255)

# Maze grid (1 = Wall, 0 = Empty path, 2 = Pellet, 3 = Ghost, 4 = Pac-Man)
maze = [
    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
    [1, 4, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 0, 1],
    [1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 1],
    [1, 0, 1, 3, 1, 0, 1, 3, 0, 0, 0, 0, 1, 3, 1, 0, 1, 3, 0, 1],
    [1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 0, 1],
    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
]

# Store pellet positions separately
pellet_positions = [(row_idx, col_idx) for row_idx, row in enumerate(maze) for col_idx, cell in enumerate(row) if cell == 2]

def display_message(screen, message, color=WHITE):
    font = pygame.font.Font(None, 50)
    text = font.render(message, True, color)
    text_rect = text.get_rect(center=(WIDTH // 2, HEIGHT // 2))
    screen.blit(text, text_rect)
    pygame.display.flip()
    pygame.time.delay(2000)

# Get Pac-Man & Ghost positions
def get_positions():
    pacman_pos = None
    ghost_positions = []
    for row_idx, row in enumerate(maze):
        for col_idx, cell in enumerate(row):
            if cell == 4:
                pacman_pos = [row_idx, col_idx]
            elif cell == 3:
                ghost_positions.append([row_idx, col_idx])
    return pacman_pos, ghost_positions

pacman_pos, ghost_positions = get_positions()
score = 0
lives = 3

def check_collision(screen):
    global lives, pacman_pos
    for ghost in ghost_positions:
        if pacman_pos == ghost:
            lives -= 1
            if lives > 0:
                display_message(screen, "You lost a life!", RED)
            if lives == 0:
                display_message(screen, "Game Over!", RED)
                pygame.quit()
                exit()
            
            # Reset Pac-Man position and update the maze immediately
            old_row, old_col = pacman_pos
            maze[old_row][old_col] = 0  # Clear old position
            pacman_pos = [1, 1]  # Reset to start position
            maze[1][1] = 4  # Place Pac-Man back on the grid

def draw_maze(screen):
    for row_idx, row in enumerate(maze):
        for col_idx, cell in enumerate(row):
            x, y = col_idx * GRID_SIZE, row_idx * GRID_SIZE
            if cell == 1:
                pygame.draw.rect(screen, BLUE, (x, y, GRID_SIZE, GRID_SIZE))
            elif cell == 2:
                pygame.draw.circle(screen, WHITE, (x + GRID_SIZE // 2, y + GRID_SIZE // 2), 5)
            elif cell == 3:
                pygame.draw.circle(screen, RED, (x + GRID_SIZE // 2, y + GRID_SIZE // 2), 12)
            elif cell == 4:
                pygame.draw.circle(screen, YELLOW, (x + GRID_SIZE // 2, y + GRID_SIZE // 2), 12)

def move_pacman(direction, screen):
    global pacman_pos, score
    row, col = pacman_pos
    new_row, new_col = row, col
    if direction == "UP":
        new_row -= 1
    elif direction == "DOWN":
        new_row += 1
    elif direction == "LEFT":
        new_col -= 1
    elif direction == "RIGHT":
        new_col += 1
    
    if maze[new_row][new_col] != 1:
        if (new_row, new_col) in pellet_positions:
            pellet_positions.remove((new_row, new_col))
            score += 10
        
        maze[row][col] = 0  # Clear old position
        maze[new_row][new_col] = 4  # Move Pac-Man
        pacman_pos = [new_row, new_col]
        
    check_collision(screen)

def move_ghosts(screen):
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    for ghost in ghost_positions:
        row, col = ghost
        random.shuffle(directions)
        
        for dr, dc in directions:
            new_row, new_col = row + dr, col + dc
            
            if maze[new_row][new_col] in [0, 2]:
                maze[row][col] = 0  # Restore empty path
                if (row, col) in pellet_positions:
                    maze[row][col] = 2  # Restore pellet if ghost was on one
                
                ghost[0], ghost[1] = new_row, new_col
                maze[new_row][new_col] = 3  # Move ghost
                break
    check_collision(screen)

def main():
    screen = pygame.display.set_mode((WIDTH, HEIGHT))
    pygame.display.set_caption("Pac-Man (Beginner Version)")
    clock = pygame.time.Clock()
    
    while True:
        screen.fill(BLACK)
        draw_maze(screen)
        font = pygame.font.Font(None, 36)
        score_text = font.render(f"Score: {score}", True, WHITE)
        lives_text = font.render(f"Lives: {lives}", True, WHITE)
        screen.blit(score_text, (10, 10))
        screen.blit(lives_text, (WIDTH - 100, 10))
        
        move_ghosts(screen)
        pygame.display.flip()
        
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                exit()
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_UP:
                    move_pacman("UP", screen)
                elif event.key == pygame.K_DOWN:
                    move_pacman("DOWN", screen)
                elif event.key == pygame.K_LEFT:
                    move_pacman("LEFT", screen)
                elif event.key == pygame.K_RIGHT:
                    move_pacman("RIGHT", screen)
        
        clock.tick(5)  # Control game speed
    
if __name__ == "__main__":
    main()
```

## Wrapping Up

Congratulations! You’ve built a **Pac-Man game** using Python and Pygame. This project introduced you to key game development concepts such as handling user input, managing game state, rendering graphics, and implementing random movement for ghosts.

**Next Steps:**

- **Add Ghost AI**: Implement pathfinding (e.g., A* algorithm) to make the ghosts smarter.
- **Add Power Pellets**: Introduce power-ups that allow Pac-Man to temporarily eat ghosts.
- **Improve Animations**: Add sprite-based animations for smoother movement.
- **Expand the Maze**: Create larger, more complex levels for added challenge.
- **Track High Scores**: Implement a scoring system that saves the player’s best runs.

By expanding on this foundation, you can create a more engaging and challenging Pac-Man experience. Keep coding, and have fun building games!

**Happy coding!**