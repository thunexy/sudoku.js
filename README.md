Sudoku.js
==========

A Sudoku puzzle **generator** and **solver** JavaScript library.

Based on ["Solving Every Sudoku Puzzle"][1] by Peter Norvig, and 
Michael Anderson's [generator/solver][2].


Intro
--------------------------------------------------------------------------------

Puzzles are represented by a string of digits, 1-9, and '.' as spaces. Each
character represents a square, e.g., 

    "52...6.........7.13...........4..8..6......5...........418.........3..2...87....."
    
Represents the following board:

    5 2 . | . . 6 | . . .   
    . . . | . . . | 7 . 1   
    3 . . | . . . | . . .   
    ------+-------+------
    . . . | 4 . . | 8 . .   
    6 . . | . . . | . 5 .   
    . . . | . . . | . . .   
    ------+-------+------
    . 4 1 | 8 . . | . . .   
    . . . | . 3 . | . 2 .   
    . . 8 | 7 . . | . . .


Generate a Sudoku puzzle
--------------------------------------------------------------------------------

Generage a Sudoku puzzle of a particular difficulty, e.g,

```javascript
>>> sudoku.generate("easy")
"672819345193..4862485..3197824137659761945283359...714.38..1426.174.6.38.463...71"

>>> sudoku.generate("medium")
"8.4.71.9.976.3....5.196....3.7495...692183...4.5726..92483591..169847...753612984"

>>> sudoku.generate("hard")
".17..69..356194.2..89..71.6.65...273872563419.43...685521......798..53..634...59."
```

Valid difficulties are as follows, and represent the number of given squares:

    "easy":         62
    "medium":       53
    "hard":         44
    "very-hard":    35
    "insane":       26
    "inhuman":      17
    
    
You may also enter a custom number of squares to give, e.g.,

```javascript
>>> sudoku.generate(60)
"8941376521532687497269548...72.9158.538.4219..19.852..3874.69.52415793689658.34.."
```

The number of givens must be a number between 17 and 81 inclusive. If it's 
outside of that range, the number of givens will be set to the closest bound, 
e.g., 0 will be treated as 17, and 100 as 81.


By default, the puzzles should have unique solutions, unless you set `unique` to
false, e.g., 

```javascript
sudoku.generate("easy", false)
```

Note: **Puzzle uniqueness is not yet implemented**, so puzzles are *not* 
guaranteed to have unique solutions.


Solve a Sudoku puzzle
--------------------------------------------------------------------------------

Solve a Sudoku puzzle given a Sudoku puzzle represented as a string, e.g.,

```javascript
>>> sudoku.solve(".17..69..356194.2..89..71.6.65...273872563419.43...685521......798..53..634...59.");
"217386954356194728489257136165948273872563419943712685521439867798625341634871592"
```


Board string ↔ grid
--------------------------------------------------------------------------------

Board string → grid:

```javascript
>>> sudoku.board_string_to_grid("23.94.67.8..3259149..76.32.1.....7925.321.4864..68.5317..1....96598721433...9...7")
[
    ["2","3",".","9","4",".","6","7","."],
    ["8",".",".","3","2","5","9","1","4"],
    ["9",".",".","7","6",".","3","2","."],
    ["1",".",".",".",".",".","7","9","2"],
    ["5",".","3","2","1",".","4","8","6"],
    ["4",".",".","6","8",".","5","3","1"],
    ["7",".",".","1",".",".",".",".","9"],
    ["6","5","9","8","7","2","1","4","3"],
    ["3",".",".",".","9",".",".",".","7"]
]
```

Board grid → string:

```javascript
>>> sudoku.board_grid_to_string([
    ["2","3",".","9","4",".","6","7","."],
    ["8",".",".","3","2","5","9","1","4"],
    ["9",".",".","7","6",".","3","2","."],
    ["1",".",".",".",".",".","7","9","2"],
    ["5",".","3","2","1",".","4","8","6"],
    ["4",".",".","6","8",".","5","3","1"],
    ["7",".",".","1",".",".",".",".","9"],
    ["6","5","9","8","7","2","1","4","3"],
    ["3",".",".",".","9",".",".",".","7"]
])
"23.94.67.8..3259149..76.32.1.....7925.321.4864..68.5317..1....96598721433...9...7"
```


Get candidates
--------------------------------------------------------------------------------

Get a grid of squares and their candidate values, propagating constraints, i.e.,
candidates restrict their peer candidates.

```javascript
>>> sudoku.get_candidates("4.25..389....4.265..523.147..1652.7.6..1945322543876915....3.1....4..9.....8....3")
[
    ["4",     "167",    "2",    "5",  "167",  "16",   "3",   "8",  "9"  ],
    ["13789", "13789",  "3789", "79", "4",    "189",  "2",   "6",  "5"  ],
    ["89",    "689",    "5",    "2",  "3",    "689",  "1",   "4",  "7"  ],
    ["389",   "389",    "1",    "6",  "5",    "2",    "48",  "7",  "48" ],
    ["6",     "78",     "78",   "1",  "9",    "4",    "5",   "3",  "2"  ],
    ["2",     "5",      "4",    "3",  "8",    "7",    "6",   "9",  "1"  ],
    ["5",     "246789", "6789", "79", "267",  "3",    "478", "1",  "468"],
    ["1378",  "123678", "3678", "4",  "1267", "156",  "9",   "25", "68" ],
    ["179",   "124679", "679",  "8",  "1267", "1569", "47",  "25", "3"  ]
]
```


Print a board to the console
----------------------------

```javascript
>>> sudoku.print_board(".17..69..356194.2..89..71.6.65...273872563419.43...685521......798..53..634...59.");
. 1 7   . . 6   9 . .   
3 5 6   1 9 4   . 2 .   
. 8 9   . . 7   1 . 6   

. 6 5   . . .   2 7 3   
8 7 2   5 6 3   4 1 9   
. 4 3   . . .   6 8 5   

5 2 1   . . .   . . .   
7 9 8   . . 5   3 . .   
6 3 4   . . .   5 9 .  
```   


References:
-----------

- ["Solving Every Sudoku Puzzle"][1] by Peter Norvig
- Michael Anderson's Python [generator/solver][2] for Mac OS X
- ["Sudoku"][3] on Wikipedia
- [95 Sudoku Puzzles][4]
- Andrew Stuart's [online Sudoku Solver][5]


[1]: http://norvig.com/sudoku.html
[2]: https://github.com/andermic/cousins/tree/master/sudoku
[3]: http://en.wikipedia.org/wiki/Sudoku
[4]: http://magictour.free.fr/top95
[5]: http://www.sudokuwiki.org/sudoku.htm
