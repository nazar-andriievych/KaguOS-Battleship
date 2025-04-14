# Battleship Game for KaguOS

## Overview
A classic two-player Battleship game implemented in KaguOS assembly language. Players take turns guessing coordinates to sink each other's fleet of ships.

## Game Features
- Two-player turn-based gameplay
- Visual battlefield rendering
- Input validation to prevent duplicate guesses
- Win condition detection
- Color-coded hit/miss feedback

## Installation & Execution

### Prerequisites
- KaguOS environment set up
- `main.disk` with sufficient free blocks

### Compilation Steps
1. Compile the game source:
   ```bash
   ./user_asm.sh battleship/battleship.kga
   ```

   Successful compilation outputs: `build/user.disk`

2. Copy to main disk (example block allocation):
   ```bash
   ./copy_file_to_disk.sh build/user.disk main.disk 566 2000
   ```

3. Run with bootloader:
   ```bash
   ./bootloader build/kernel.disk 10000
   ```

## Game Setup
1. **Battlefield Files**:
   * Requires two text files (`field1.txt` and `field2.txt`)
   * 10x10 grids with 'X' marking ship positions
   * Example format:
     ```
     ccccXccccc
     cccccccccc
     ccXXcccccc
     ... [10 lines]
     ```

2. **Memory Allocation**:
   * Automatically checks for sufficient RAM
   * Allocates separate memory regions for:
     * Player 1's view of Player 2's board
     * Player 2's view of Player 1's board
     * Game state tracking

## Gameplay Instructions

### Turn Sequence
1. Current player's turn is displayed
2. The opponent's board is rendered showing:
   * 'r' (red) for hits
   * 'b' (blue) for misses
   * 'c' (default) for unexplored cells
3. Player enters coordinates in `row,column` format (e.g., `3,7`)
4. System validates input:
   * Checks for valid format
   * Ensures coordinate wasn't previously guessed
   * Provides error messages for invalid input
5. Hit detection:
   * Successful hit: Cell turns red, hit counter increments
   * Miss: Cell turns blue
6. Turn continues until player misses

### Winning Condition
* First player to sink all 20 ship cells wins
* Victory message displays for the winning player

## Technical Implementation

### Key Components
1. **Battlefield Analysis**:
   * Parses ship positions from text files
   * Stores coordinates in memory as space-separated strings
2. **Memory Management**:
   * Dynamic allocation checking
   * Bitmap rendering system calls
3. **Game Loop**:
   * Player turn switching
   * Hit/miss detection
   * Win condition checking

### System Calls Used
* `SYS_CALL_RENDER_BITMAP`: Display game boards
* `SYS_CALL_READ_INPUT`: Get player coordinates
* `SYS_CALL_PRINTLN`: Display messages
* `SYS_CALL_SLEEP`: Pause for message readability

### Error Handling
* Invalid battlefield files terminate with error
* Memory allocation failures handled gracefully
* Input validation prevents game crashes

## Sample Game Session
```
Welcome to Battleship game!
Player1 turn
Player2 field
[rendered board]
Player1: Shoot a boat by writing coordinates(row,column): 3,5
You hit a ship!
Player1 turn
... [game continues] ...
Player 1 wins!
```

## Customization
To modify the game:
1. Adjust battlefield files (`field1.txt`, `field2.txt`)
2. Change ship count by modifying `all_ships_cells` variable
3. Update colors by changing 'r'/'b' markers

## Troubleshooting
* "Not a valid source file": Ensure battlefield files exist
* Memory errors: Increase RAM allocation in bootloader
* Rendering issues: Verify bitmap address ranges
