# ARCHITECTURE.md

Written by team-lead before spawning teammates. This is the shared blueprint —
teammates read it to understand what they are building and how their module fits.
Update it when the structure changes; do not let it drift from the actual code.

## Module Structure

- `src/board.py`: 3x3 grid state management, move validation, win detection
- `src/game.py`: turn management, player state, game loop coordination
- `src/ui.py`: terminal rendering, user input parsing and validation
- `src/main.py`: application entry point, wires all components together

## Interfaces

### board.py
- `Board` class:
  - `__init__()` - Initialize empty 3x3 grid
  - `display()` - Return string representation of board for UI
  - `is_valid(row, col)` -> bool - Check if move is valid (within bounds, cell empty)
  - `apply_move(row, col, player)` -> None - Place player's mark (X or O)
  - `check_winner()` -> str | None - Return 'X', 'O', or None if no winner
  - `is_full()` -> bool - Check if board is completely filled
  - `reset()` -> None - Clear board for new game

### game.py
- `Game` class:
  - `__init__(board: Board)` - Initialize with board reference
  - `current_player()` -> str - Return current player ('X' or 'O')
  - `make_move(row, col)` -> dict - Attempt move, return result with status
  - `check_game_status()` -> dict - Check winner, draw, or continue
  - `reset()` -> None - Reset board and starting player
  - `switch_player()` -> None - Toggle current player

### ui.py
- `UI` class:
  - `display_board(board: Board)` - Render current board state
  - `display_message(msg: str)` - Show game messages
  - `get_player_input()` -> tuple | None - Parse row,col input, handle quit
  - `display_winner(player: str)` - Announce winner
  - `display_draw()` - Announce draw
  - `prompt_play_again()` -> bool - Ask if player wants new game

### main.py
- Application entry point:
  - Create Board, Game, and UI instances
  - Run game loop
  - Handle restart logic
  - Graceful exit

## Shared Data Structures

### Move Result
```python
{
    "success": bool,      # Whether move was valid and applied
    "message": str,       # Status message for UI
    "game_over": bool,    # Whether game ended
    "winner": str | None  # 'X', 'O', or None
}
```

### Game Status
```python
{
    "game_over": bool,
    "winner": str | None,
    "draw": bool,
    "current_player": str
}
```

### Input Format
- User enters: `"row,col"` (e.g., `"1,2"`)
- Rows/columns are 1-indexed for user-friendliness
- Parse to 0-indexed internally: `row-1, col-1`

## External Dependencies

- **Standard library only**: No external dependencies required
  - Uses: `sys` for clean exit
  - Uses: `typing` for type hints
