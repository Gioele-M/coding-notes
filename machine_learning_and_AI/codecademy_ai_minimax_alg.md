# Minimax

The minimax algorithm is a decision making algorithm that is used for finding the best move in a two player game. It is a recursive algorithm.

The algorithm has an evaluation function which determines the best outcome 

IF you want a copy of something use DEEPCOPY()
Max value is float('Inf')



It is basically a transverision algorithm

You need to declare all things that can happen in the game:
Ex. Tic-Tac-Toe
- Print board
- Select space -> determine if space is occupied (returns True or False)
- Available moves -> determine if any moves are left
- Has won -> check if the game is over

Minimax function:
- Evaluates if the game is over at every iteration
- Evaluates which of the player is playing, tries to reduce the damage as much as possible
	+ If it is the cpu turn 
	
```py
def minimax(input_board, is_maximizing): #is maximising is true if you want to win, false else
  # Base case - the game is over, so we return the value of the board
  if game_is_over(input_board):
    return [evaluate_board(input_board), '']

  best_move = ''
  if is_maximizing == True:
    best_value = -float("Inf")
    symbol = "X"
  else:
    best_value = float("Inf")
    symbol = "O"
  for move in available_moves(input_board):
    new_board = deepcopy(input_board)
    select_space(new_board, move, symbol)
    hypothetical_value = minimax(new_board, not is_maximizing)[0]
    if is_maximizing == True and hypothetical_value > best_value:
      best_value = hypothetical_value
      best_move = move
    if is_maximizing == False and hypothetical_value < best_value:
      best_value = hypothetical_value
      best_move = move
  return [best_value, best_move]
```



For more complicated games, such as connect four or chess, we need to stop the recursion before the algorithm expands too much
	! Add another parameter called depth that works as stopping condition and reduces in value at each iteration
	! Given that this does not allow to get into the leaves of the tree, we need another way of determining what is the right move to do



# Alpha Beta pruning
In order to transverse the tree leaves without increasing the running time exponentially we use the technique called alpha-beta pruning
	- Basically we ignore parts of tree that we know will be a dead end

Algorithm:
We keep track of two variables in each node, alpha and beta
- Alpha -> keeps track of the minimum score the maximizing player can possibly get (starts a -inf)
- Beta -> keeps track of the maximum score obtainable (starts at inf)

For Each Node:
	IF Alpha >= Beta:
		Stop looking at the node

Reset alpha and beta:
if new found best_value >(greater than) alpha -> alpha = -inf
if new found best_value < beta -> beta = inf


```py


def minimax(input_board, is_maximizing, depth, alpha, beta):
  # Base case - the game is over, so we return the value of the board
  if game_is_over(input_board) or depth == 0:
    return [evaluate_board(input_board), ""]
  best_move = ""
  if is_maximizing == True:
    best_value = -float("Inf")
    symbol = "X"
  else:
    best_value = float("Inf")
    symbol = "O"
  for move in available_moves(input_board):
    new_board = deepcopy(input_board)
    select_space(new_board, move, symbol)
    hypothetical_value = minimax(new_board, not is_maximizing, depth - 1, alpha, beta)[0]
    if is_maximizing == True and hypothetical_value > best_value:
      best_value = hypothetical_value
      best_move = move
      #set alpha as maximum between the two
      alpha = max(alpha, best_value)
    if is_maximizing == False and hypothetical_value < best_value:
      best_value = hypothetical_value
      best_move = move
      beta = min(beta, best_value)
    
    if alpha >= beta:
      break
  return [best_value, best_move, alpha, beta] #shouldnt need alpha and beta

```












