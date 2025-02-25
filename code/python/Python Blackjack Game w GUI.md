# Build a Python Blackjack Game + GUI with PyQt (Step-by-Step)

In this tutorial, we will build a simple yet interactive **Blackjack game** using **Python** and **PyQt5**. This project will help you understand **Object-Oriented Programming (OOP)**, **GUI design**, and **event handling** in PyQt5.

By the end, you’ll have a fully functional game where you can play a round of Blackjack against the dealer using an intuitive GUI.

This tutorial covers:

- Using PyQt5 to create an interactive GUI
- Implementing game logic for Blackjack
- Handling user interactions with buttons
- Dynamically updating the game state
- Using OOP principles to organize your code

Let's dive in!

## Step 1: Setting Up the Project

Before we start coding, let’s set up our [Python project](https://hackr.io/blog/python-projects):

1. Make sure Python is installed on your computer. If not, download it from the [official Python website](https://www.python.org/).  
2. Open your favorite code editor or IDE.  
3. Create a new Python file, for example, `blackjack.py`.

Before we start coding, **Install PyQt5** if you haven't already. You can install it using:

```python
pip install PyQt5
```

Great, now, let's dive head first into our [Python editor](https://app.hackr.io/editors/python) to get this build started.

## Step 2: Understanding How Blackjack Works

Blackjack is a popular card game where:

- Players try to get as close to **21** as possible without exceeding it.
- **Face cards** (J, Q, K) are worth **10 points**, and **Aces** can be **1 or 11**.
- The dealer must hit until they reach at least **17**.
- The player can choose to **hit** (take another card) or **stand** (keep their current total).

We’ll use [PyQt5 to create an interactive interface](https://hackr.io/blog/how-to-use-python-pyqt) with labeled cards and buttons for **Hit, Stand, and Restart**.

When we're done, we should have something that looks like this:

![GUI for Blackjack game built with PyQt in Python.](https://i.imgur.com/ZipBnek.gif)

## Step 3: Importing Required Modules

Now, let’s start coding our Python blackjack game! We need several Python modules to build the game logic and graphical interface:

```python
import sys  # For handling system operations
import random  # For shuffling and dealing cards randomly
from PyQt5.QtWidgets import QApplication, QWidget, QLabel, QPushButton, QVBoxLayout, QHBoxLayout  
from PyQt5.QtGui import QPixmap, QFont  
from PyQt5.QtCore import Qt  
```

Why Do We Use These Modules?

- **sys**: Required for running the PyQt5 application.
- **random**: We use [Python random](https://hackr.io/blog/python-random-function) to shuffle the deck and deal cards randomly.
- **PyQt5.QtWidgets**: Provides the necessary GUI components, such as buttons, labels, and layout managers.
- **PyQt5.QtGui**: Allows us to use images (`QPixmap`) for card graphics and set custom fonts (`QFont`).
- **PyQt5.QtCore**: Includes core features like alignment and window behavior (`Qt`).

With these modules, we can build a functional and visually appealing blackjack game!

## Step 4: Creating the Class Skeleton

To follow good [Python OOP](https://hackr.io/blog/python-object-oriented-programming) design practices, we will first create the class skeleton and then gradually implement each method. Our Blackjack game consists of two main classes:

1. **Card Class**: Represents an individual playing card with a face, value, and suit.
2. **BlackjackGame Class**: Handles the game logic and GUI.

```python
# Card class
class Card:
    def __init__(self, face, value, suit):
        self.face = face
        self.value = value
        self.suit = suit

# Blackjack GUI class
class BlackjackGame(QWidget):
    def __init__(self):
        super().__init__()
        self.initUI()
    
    def initUI(self):
        pass
    
    def init_deck(self):
        pass
    
    def restart(self):
        pass
    
    def deal_card(self):
        pass
    
    def calculate_score(self, cards):
        pass
    
    def update_display(self):
        pass
    
    def clear_layout(self, layout):
        pass
    
    def hit(self):
        pass
    
    def stand(self):
        pass

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = BlackjackGame()
    window.show()
    sys.exit(app.exec_())
```

**Explanation:**

- **Card Class**:
    
    - Represents a single playing card, storing its **face** (e.g., 'A', 'K', '2'), **value** (e.g., 11, 10, 2), and **suit** (♠, ♥, ♦, ♣).
- **BlackjackGame Class** (subclass of `QWidget`):
    
    - **`__init__` Method**: Initializes the game window and calls `initUI()`.
    - **`initUI` Method**: Sets up the graphical interface (buttons, labels, layouts).
    - **`init_deck` Method**: Creates and shuffles a full deck of cards.
    - **`restart` Method**: Resets the game state for a new round.
    - **`deal_card` Method**: Draws a card from the deck.
    - **`calculate_score` Method**: Computes the player's or dealer's total score, handling Ace adjustments.
    - **`update_display` Method**: Updates the UI with the latest game status.
    - **`clear_layout` Method**: Clears old UI elements before updating the display.
    - **`hit` Method**: Handles the player's "Hit" action.
    - **`stand` Method**: Handles the player's "Stand" action and lets the dealer play.
- **Main Block (`if __name__ == '__main__':`)**:
    
    - Runs the PyQt5 application, initializes the game window, and starts the event loop.

This skeleton provides the structure for our game, and we will now implement each method step by step.

## Step 5: Designing the Game Layout

We will now implement `initUI()` to define the GUI layout using `QVBoxLayout` and `QHBoxLayout`.

```python
def initUI(self):
        self.setWindowTitle("Blackjack - PyQt5 GUI")
        self.setGeometry(200, 200, 600, 400)
        
        # Layouts
        self.vbox = QVBoxLayout()
        self.dealer_box = QHBoxLayout()
        self.player_box = QHBoxLayout()
        self.controls = QHBoxLayout()
        
        # Labels for cards
        self.dealer_label = QLabel("Dealer's Cards:")
        self.player_label = QLabel("Player's Cards:")
        self.result_label = QLabel("Game in Progress")
        self.result_label.setAlignment(Qt.AlignCenter)
        self.dealer_score_label = QLabel("Dealer Score: 0")
        self.player_score_label = QLabel("Player Score: 0")

        # Align dealer/player labels and scores
        self.dealer_layout = QHBoxLayout()
        self.dealer_layout.addWidget(self.dealer_label)
        self.dealer_layout.addStretch()
        self.dealer_layout.addWidget(self.dealer_score_label)
        
        self.player_layout = QHBoxLayout()
        self.player_layout.addWidget(self.player_label)
        self.player_layout.addStretch()
        self.player_layout.addWidget(self.player_score_label)
        
        # Buttons
        self.hit_btn = QPushButton("Hit")
        self.stand_btn = QPushButton("Stand")
        self.restart_btn = QPushButton("Restart")
        
        self.hit_btn.clicked.connect(self.hit)
        self.stand_btn.clicked.connect(self.stand)
        self.restart_btn.clicked.connect(self.restart)
        
        # Add widgets to layouts
        self.vbox.addLayout(self.dealer_layout)
        self.vbox.addLayout(self.dealer_box)
        self.vbox.addLayout(self.player_layout)
        self.vbox.addLayout(self.player_box)
        self.vbox.addWidget(self.result_label)
        
        self.controls.addWidget(self.hit_btn)
        self.controls.addWidget(self.stand_btn)
        self.controls.addWidget(self.restart_btn)
        
        self.vbox.addLayout(self.controls)
        
        self.setLayout(self.vbox)
        self.restart()
```

**Breakdown:**

- **Window Setup**:
    
    - The main window title is set to `"Blackjack - PyQt5 GUI"`.
    - The window size is defined as **600x400 pixels**.
- **Layouts Used**:
    
    - **`QVBoxLayout`**: Arranges elements vertically (dealer area, player area, and controls).
    - **`QHBoxLayout`**: Used for arranging dealer/player labels, scores, and controls horizontally.
- **Labels for Cards**:
    
    - Dealer and player labels (`QLabel`) display **"Dealer's Cards:"** and **"Player's Cards:"**.
    - **Result Label (`QLabel`)** displays the game status (e.g., **"Game in Progress"**).
    - Score labels show the **dealer’s and player’s current score**.
- **Buttons for Gameplay**:
    
    - **Hit (`QPushButton`)**: Adds a card to the player’s hand.
    - **Stand (`QPushButton`)**: Ends the player's turn and lets the dealer play.
    - **Restart (`QPushButton`)**: Resets the game.
    - These buttons are linked to their respective methods using `.clicked.connect()`.
- **Adding Widgets to Layouts**:
    
    - Dealer and player sections are grouped using **horizontal layouts**.
    - Buttons are aligned in a **horizontal layout** (`controls`).
    - The main layout (`vbox`) stacks everything neatly.
- **Calling `restart()`**:
    
    - This ensures the game starts in a **fresh state** when the window loads.

This layout provides a **clean, structured, and interactive** GUI for the Blackjack game.

## Step 6: Initializing the Deck

The `init_deck()` method is responsible for creating a standard 52-card deck, assigning values to the cards, and shuffling them before use.

```python
def init_deck(self):
        suits = ['♠', '♥', '♦', '♣']
        faces = {'A': 11, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9, '10': 10, 'J': 10, 'Q': 10, 'K': 10}
        deck = [Card(face, value, suit) for suit in suits for face, value in faces.items()]
        random.shuffle(deck)
        return deck
```

**Breakdown:**

- **Defining Suits**:
    
    - The four suits in a standard deck are **Spades (♠), Hearts (♥), Diamonds (♦), and Clubs (♣)**.
- **Defining Face Values**:
    
    - Cards 2-10 retain their numeric value.
    - Face cards **(J, Q, K)** are assigned a value of **10**.
    - The Ace **(A)** is given an initial value of **11**, but its value will be adjusted later if needed.
- **Creating the Deck**:
    
    - We use a [list comprehension](https://hackr.io/blog/python-list-comprehensions) to generate all **52 cards**, combining each suit with all face values.
    - Each card is an instance of the **`Card` class**, which stores its **face, value, and suit**.
- **Shuffling the Deck**:
    
    - The deck is **randomly shuffled** using `random.shuffle(deck)`, ensuring that each game starts with a randomized deck.

This method ensures that every new game begins with a freshly shuffled deck of **52 cards**, maintaining the randomness essential to Blackjack gameplay.

## Step 7: Restarting the Game

The `restart()` method resets the game state, clears previous hands, shuffles a new deck, and deals the initial two cards to both the player and dealer.

```python
def restart(self):
        self.deck = self.init_deck()
        self.player_cards = []
        self.dealer_cards = []
        self.player_score = 0
        self.dealer_score = 0
        self.game_over = False  # Reset game state

        
        # Clear previous hands
        self.clear_layout(self.dealer_box)
        self.clear_layout(self.player_box)
        
        # Deal initial cards
        for _ in range(2):
            self.player_cards.append(self.deal_card())
            self.dealer_cards.append(self.deal_card())
        
        self.update_display()
        self.hit_btn.setDisabled(False)
        self.stand_btn.setDisabled(False)
        self.result_label.setText("Game in Progress")
```

**Breakdown:**

- **Initializing a New Game**:
    
    - Calls `init_deck()` to create and shuffle a fresh deck.
    - Resets the **player's and dealer's hands** (`player_cards` and `dealer_cards`).
    - Resets **player and dealer scores** to **0**.
    - **`game_over`** is set to `False`, ensuring the game is active.
- **Clearing the Previous Hands**:
    
    - Calls `clear_layout()` on `dealer_box` and `player_box` to remove previous card visuals from the UI.
- **Dealing Initial Cards**:
    
    - Each player receives **two starting cards** using the `deal_card()` method.
- **Updating the UI**:
    
    - Calls `update_display()` to reflect the new hands on the screen.
    - **Re-enables** the "Hit" and "Stand" buttons so the player can interact with the game.
    - Updates the **result label** to display **"Game in Progress"**.

This method ensures that every time a new round starts, the game resets completely—allowing for a fresh and fair playthrough.

## Step 8: Dealing Cards and Clearing Layouts

The `deal_card()` method handles drawing a card from the deck, while `clear_layout()` ensures the UI updates properly by removing previous elements.

**Dealing a Card**

```python
def deal_card(self):
        return self.deck.pop()
```

**Clearing the Layout**

```python
def clear_layout(self, layout):
        while layout.count():
            item = layout.takeAt(0)
            widget = item.widget()
            if widget is not None:
                widget.deleteLater()
```

**Breakdown:**

**`deal_card()` Method:**

- Retrieves (`pop()`) the **top card** from the deck.
- Since the deck is shuffled at the start (`init_deck()`), this ensures randomness.
- The returned `Card` object contains the **face, value, and suit**.

**`clear_layout()` Method:**

- Loops through all widgets inside a given **layout**.
- Uses `.takeAt(0)` to remove items **one by one**.
- If the item is a **widget**, it is deleted (`deleteLater()`) to clear the UI properly.
- This is important for refreshing the **dealer's and player's hands** before updating the display.

**Why These Methods Are Important**

- `deal_card()` ensures a fair and shuffled card distribution.
- `clear_layout()` prevents overlapping UI elements when updating the card display.
- Both methods work together to maintain a clean **game flow and user experience**.

These functions help manage the core mechanics of **drawing new cards** and **resetting the game screen** smoothly.

## Step 9: Updating the Display

The `update_display()` method updates the UI by rendering the current state of the game, displaying the cards, scores, and handling game-over scenarios.

```python
def update_display(self):
        self.clear_layout(self.dealer_box)
        self.clear_layout(self.player_box)
        
        for i, card in enumerate(self.dealer_cards):
            color = "red" if card.suit in ['♥', '♦'] else "black"
            if i == 0 and not self.game_over:
                color = "blue"  # Hide the dealer's first card with blue color
                label_text = "🂠"
            else:
                label_text = f"{card.face}{card.suit}"
            
            label = QLabel(label_text)
            label.setFont(QFont("Arial", 16, QFont.Bold))
            label.setAlignment(Qt.AlignCenter)
            label.setStyleSheet(f"border: 2px solid black; padding: 10px; background: white; color: {color}; min-width: 60px; min-height: 100px;")
            self.dealer_box.addWidget(label)
        
        for card in self.player_cards:
            color = "red" if card.suit in ['♥', '♦'] else "black"
            label = QLabel(f"{card.face}{card.suit}")
            label.setFont(QFont("Arial", 16, QFont.Bold))
            label.setAlignment(Qt.AlignCenter)
            label.setStyleSheet(f"border: 2px solid black; padding: 10px; background: white; color: {color}; min-width: 60px; min-height: 100px;")
            self.player_box.addWidget(label)
        
        self.player_score = self.calculate_score(self.player_cards)
        self.dealer_score = self.calculate_score(self.dealer_cards)

        self.player_score_label.setText(f"Player Score: {self.player_score}")
        self.dealer_score_label.setText(f"Dealer Score: {self.dealer_score if self.game_over else '?'}")

        if self.player_score > 21:
            self.result_label.setText("Player Busts! Dealer Wins!")
            self.hit_btn.setDisabled(True)
            self.stand_btn.setDisabled(True)
            self.game_over = True
```

**Breakdown:**

**Clearing Previous Cards**

- Calls `clear_layout(self.dealer_box)` and `clear_layout(self.player_box)` to remove previously displayed cards before updating the UI.

**Displaying the Dealer’s Cards**

- **First Card Hidden**:
    - If the dealer’s first card is hidden (`not self.game_over`), it is displayed as **"🂠"** (face-down card).
    - Its color is set to **blue** for differentiation.
- **Other Cards Displayed**:
    - If the game is over, all dealer’s cards are revealed.
    - The text format `{card.face}{card.suit}` is used.
    - Cards in **Hearts (♥) and Diamonds (♦)** are colored **red**, while others remain **black**.

**Displaying the Player’s Cards**

- Player's cards are always visible.
- Cards are formatted as [f-strings](https://hackr.io/blog/python-f-strings) with `{face}{suit}` with color adjustments.

**Updating Scores**

- Calls `calculate_score()` to update the player's and dealer's scores.
- **Player’s Score**: Always displayed.
- **Dealer’s Score**: Hidden (`"?"`) until the game ends.

**Handling Game Over (Bust Condition)**

- If the **player's score exceeds 21**, they **bust**:
    - Updates `result_label` to **"Player Busts! Dealer Wins!"**
    - Disables **"Hit"** and **"Stand"** buttons.
    - Marks `game_over = True`.

**Why This Method is Important**

- Ensures that the **game UI updates dynamically**.
- Controls how **cards are displayed and hidden**.
- Implements **Blackjack rules** like hiding the dealer’s first card.
- Handles **busting conditions** efficiently.

This function plays a key role in the **visual and interactive aspects** of the Blackjack game.

## Step 10: Calculating the Score

The `calculate_score()` method computes the total value of a given hand while handling **Ace adjustments** to prevent busting.

```python
def calculate_score(self, cards):
        score = sum(card.value for card in cards)
        aces = sum(1 for card in cards if card.face == 'A')
        while score > 21 and aces:
            score -= 10
            aces -= 1
        return score
```

**Breakdown:**

**Summing Card Values**

- Uses **list comprehension** to sum up the values of all cards in the given hand.
- Since **face cards (J, Q, K)** have a value of **10**, and **Aces (A)** initially have **11**, they are already accounted for in the `Card` class.

**Handling Aces**

- If the **total score exceeds 21**, we check for **Aces** (`'A'`).
- Since Aces can be either **11 or 1**, we **subtract 10** from the score for each Ace **until the score is 21 or lower**.
- This ensures the **best possible score** without busting.

**Example Scenarios:**

1. **Hand: ['A', 'K'] → (11 + 10) = 21** ✅ **(Blackjack)**
2. **Hand: ['A', '5', '6'] → (11 + 5 + 6) = 22** ❌ (Bust)
    - Converts **A → 1** → New score: **(1 + 5 + 6) = 12** ✅
3. **Hand: ['A', 'A', '8'] → (11 + 11 + 8) = 30** ❌ (Bust)
    - First **A → 1** → (1 + 11 + 8) = 20 ✅

**Why This Method is Important**

- Ensures **optimal scoring**, allowing the **best Ace adjustment**.
- Prevents unnecessary busting when **multiple Aces** are present.
- Handles **edge cases** while keeping calculations **efficient**.

This function is **essential** for enforcing Blackjack rules and determining **winners and busts** accurately.

## Step 11: Implementing Player Actions – Hit and Stand

The `hit()` and `stand()` methods define how the player interacts with the game, following **Blackjack rules**.

**Handling the Player’s Turn – `hit()`**

```python
def hit(self):
        if not self.game_over:
            self.player_cards.append(self.deal_card())
            self.player_score = self.calculate_score(self.player_cards)
            self.update_display()
            
            if self.player_score > 21:
                self.result_label.setText("Player Busts! Dealer Wins!")
                self.hit_btn.setDisabled(True)
                self.stand_btn.setDisabled(True)
                self.game_over = True
                self.update_display()
```

**Breakdown:**

- Ensures the game is still in progress (`not self.game_over`).
- **Deals a new card** to the player (`self.deal_card()`).
- **Recalculates the player’s score** (`calculate_score()`).
- Calls `update_display()` to **refresh the UI**.
- **Bust Condition:**
    - If the player's score **exceeds 21**, the game declares a **bust**.
    - Updates `result_label` to **"Player Busts! Dealer Wins!"**.
    - **Disables** the "Hit" and "Stand" buttons.
    - Marks the game as **over (`self.game_over = True`)**.

**Handling the Dealer’s Turn – `stand()`**

```python
def stand(self):
        self.hit_btn.setDisabled(True)
        self.stand_btn.setDisabled(True)
        
        while self.dealer_score < 17:
            self.dealer_cards.append(self.deal_card())
            self.dealer_score = self.calculate_score(self.dealer_cards)
        
        self.game_over = True
        self.update_display()
        
        if self.dealer_score > 21:
            self.result_label.setText("Dealer Busts! Player Wins!")
        elif self.dealer_score > self.player_score:
            self.result_label.setText("Dealer Wins!")
        elif self.dealer_score < self.player_score:
            self.result_label.setText("Player Wins!")
        else:
            self.result_label.setText("It's a Tie!")
        QApplication.processEvents()
```

**Breakdown:**

- **Disables the "Hit" and "Stand" buttons** to prevent further interaction.
- A [while loop](https://hackr.io/blog/python-while-loop) ensures the dealer **continues drawing cards until their score reaches at least 17**, following **Blackjack rules**.
- Once the dealer stops drawing:
    - The game is **marked as over (`self.game_over = True`)**.
    - Calls `update_display()` to **reveal all dealer cards**.
- Determines the **final game result**:
    - If the dealer **busts (score > 21)** → **Player wins**.
    - If the dealer **has a higher score** → **Dealer wins**.
    - If the player **has a higher score** → **Player wins**.
    - If scores are **equal**, it's a **tie**.
- **Ensures UI updates smoothly** using `QApplication.processEvents()`.

Why These Methods Are Important

- Implements **core Blackjack mechanics** – player actions and dealer rules.  
- Ensures **fair and rule-compliant gameplay**.  
- Handles **bust scenarios and game endings** dynamically.  
- Disables inputs after game conclusion to **prevent further actions**.

These two methods **complete the game logic**, allowing smooth and interactive **Blackjack gameplay**.

## Final Code: Blackjack Game

```python
import sys
import random
from PyQt5.QtWidgets import QApplication, QWidget, QLabel, QPushButton, QVBoxLayout, QHBoxLayout
from PyQt5.QtGui import QPixmap, QFont
from PyQt5.QtCore import Qt

# Card class
class Card:
    def __init__(self, face, value, suit):
        self.face = face
        self.value = value
        self.suit = suit

# Blackjack GUI class
class BlackjackGame(QWidget):
    def __init__(self):
        super().__init__()
        self.initUI()

    def initUI(self):
        self.setWindowTitle("Blackjack - PyQt5 GUI")
        self.setGeometry(200, 200, 600, 400)
        
        # Layouts
        self.vbox = QVBoxLayout()
        self.dealer_box = QHBoxLayout()
        self.player_box = QHBoxLayout()
        self.controls = QHBoxLayout()
        
        # Labels for cards
        self.dealer_label = QLabel("Dealer's Cards:")
        self.player_label = QLabel("Player's Cards:")
        self.result_label = QLabel("Game in Progress")
        self.result_label.setAlignment(Qt.AlignCenter)
        self.dealer_score_label = QLabel("Dealer Score: 0")
        self.player_score_label = QLabel("Player Score: 0")

        # Align dealer/player labels and scores
        self.dealer_layout = QHBoxLayout()
        self.dealer_layout.addWidget(self.dealer_label)
        self.dealer_layout.addStretch()
        self.dealer_layout.addWidget(self.dealer_score_label)
        
        self.player_layout = QHBoxLayout()
        self.player_layout.addWidget(self.player_label)
        self.player_layout.addStretch()
        self.player_layout.addWidget(self.player_score_label)
        
        # Buttons
        self.hit_btn = QPushButton("Hit")
        self.stand_btn = QPushButton("Stand")
        self.restart_btn = QPushButton("Restart")
        
        self.hit_btn.clicked.connect(self.hit)
        self.stand_btn.clicked.connect(self.stand)
        self.restart_btn.clicked.connect(self.restart)
        
        # Add widgets to layouts
        self.vbox.addLayout(self.dealer_layout)
        self.vbox.addLayout(self.dealer_box)
        self.vbox.addLayout(self.player_layout)
        self.vbox.addLayout(self.player_box)
        self.vbox.addWidget(self.result_label)
        
        self.controls.addWidget(self.hit_btn)
        self.controls.addWidget(self.stand_btn)
        self.controls.addWidget(self.restart_btn)
        
        self.vbox.addLayout(self.controls)
        
        self.setLayout(self.vbox)
        self.restart()
    
    def init_deck(self):
        suits = ['♠', '♥', '♦', '♣']
        faces = {'A': 11, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9, '10': 10, 'J': 10, 'Q': 10, 'K': 10}
        deck = [Card(face, value, suit) for suit in suits for face, value in faces.items()]
        random.shuffle(deck)
        return deck
    
    def restart(self):
        self.deck = self.init_deck()
        self.player_cards = []
        self.dealer_cards = []
        self.player_score = 0
        self.dealer_score = 0
        self.game_over = False  # Reset game state

        
        # Clear previous hands
        self.clear_layout(self.dealer_box)
        self.clear_layout(self.player_box)
        
        # Deal initial cards
        for _ in range(2):
            self.player_cards.append(self.deal_card())
            self.dealer_cards.append(self.deal_card())
        
        self.update_display()
        self.hit_btn.setDisabled(False)
        self.stand_btn.setDisabled(False)
        self.result_label.setText("Game in Progress")
    
    def deal_card(self):
        return self.deck.pop()
    
    def calculate_score(self, cards):
        score = sum(card.value for card in cards)
        aces = sum(1 for card in cards if card.face == 'A')
        while score > 21 and aces:
            score -= 10
            aces -= 1
        return score

    def update_display(self):
        self.clear_layout(self.dealer_box)
        self.clear_layout(self.player_box)
        
        for i, card in enumerate(self.dealer_cards):
            color = "red" if card.suit in ['♥', '♦'] else "black"
            if i == 0 and not self.game_over:
                color = "blue"  # Hide the dealer's first card with blue color
                label_text = "🂠"
            else:
                label_text = f"{card.face}{card.suit}"
            
            label = QLabel(label_text)
            label.setFont(QFont("Arial", 16, QFont.Bold))
            label.setAlignment(Qt.AlignCenter)
            label.setStyleSheet(f"border: 2px solid black; padding: 10px; background: white; color: {color}; min-width: 60px; min-height: 100px;")
            self.dealer_box.addWidget(label)
        
        for card in self.player_cards:
            color = "red" if card.suit in ['♥', '♦'] else "black"
            label = QLabel(f"{card.face}{card.suit}")
            label.setFont(QFont("Arial", 16, QFont.Bold))
            label.setAlignment(Qt.AlignCenter)
            label.setStyleSheet(f"border: 2px solid black; padding: 10px; background: white; color: {color}; min-width: 60px; min-height: 100px;")
            self.player_box.addWidget(label)
        
        self.player_score = self.calculate_score(self.player_cards)
        self.dealer_score = self.calculate_score(self.dealer_cards)

        self.player_score_label.setText(f"Player Score: {self.player_score}")
        self.dealer_score_label.setText(f"Dealer Score: {self.dealer_score if self.game_over else '?'}")

        if self.player_score > 21:
            self.result_label.setText("Player Busts! Dealer Wins!")
            self.hit_btn.setDisabled(True)
            self.stand_btn.setDisabled(True)
            self.game_over = True

    def clear_layout(self, layout):
        while layout.count():
            item = layout.takeAt(0)
            widget = item.widget()
            if widget is not None:
                widget.deleteLater()

    def hit(self):
        if not self.game_over:
            self.player_cards.append(self.deal_card())
            self.player_score = self.calculate_score(self.player_cards)
            self.update_display()
            
            if self.player_score > 21:
                self.result_label.setText("Player Busts! Dealer Wins!")
                self.hit_btn.setDisabled(True)
                self.stand_btn.setDisabled(True)
                self.game_over = True
                self.update_display()

    def stand(self):
        self.hit_btn.setDisabled(True)
        self.stand_btn.setDisabled(True)
        
        while self.dealer_score < 17:
            self.dealer_cards.append(self.deal_card())
            self.dealer_score = self.calculate_score(self.dealer_cards)
        
        self.game_over = True
        self.update_display()
        
        if self.dealer_score > 21:
            self.result_label.setText("Dealer Busts! Player Wins!")
        elif self.dealer_score > self.player_score:
            self.result_label.setText("Dealer Wins!")
        elif self.dealer_score < self.player_score:
            self.result_label.setText("Player Wins!")
        else:
            self.result_label.setText("It's a Tie!")
        QApplication.processEvents()
    
if __name__ == "__main__":
    app = QApplication(sys.argv)
    game = BlackjackGame()
    game.show()
    sys.exit(app.exec_())
```

## Wrapping Up

Congratulations! You've successfully built a **Blackjack game** using **Python and PyQt5**. This project demonstrated how to:

- Use **PyQt5** to create an interactive GUI.  
- Implement **Blackjack game logic** with object-oriented programming.  
- Manage **card drawing, scoring, and game rules** dynamically.  
- Handle **user input and UI updates** for a smooth gaming experience.

**Next Steps:**

**Enhance the UI** – Add custom card images or a more stylish layout.  
**Implement a dealer AI** – Add more sophisticated decision-making for the dealer.  
**Track Player Statistics** – Show win/loss records over multiple rounds.  
**Add Sound Effects** – Play sound effects for card dealing and game events.

With these improvements, you can take your **Blackjack game to the next level!**

Happy coding, and may the best hand win! 🃏♠️♦️♣️