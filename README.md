//Tic-Tac-Toe Game with AI
#include <iostream>
#include <vector>

using namespace std;

// Global 3x3 board
char board[3][3] = {
    {'1', '2', '3'},
    {'4', '5', '6'},
    {'7', '8', '9'}
};

// Function to display the current state of the board
void displayBoard() {
    cout << "Current board:\n";
    for (int i = 0; i < 3; i++) {
        cout << " " << board[i][0] << " | " << board[i][1] << " | " << board[i][2] << "\n";
        if (i < 2) cout << "---|---|---\n";
    }
}

// Function to check if a player has won
bool checkWin(char player) {
    // Check rows and columns for a win
    for (int i = 0; i < 3; i++) {
        if ((board[i][0] == player && board[i][1] == player && board[i][2] == player) ||
            (board[0][i] == player && board[1][i] == player && board[2][i] == player))
            return true;
    }
    // Check diagonals for a win
    if ((board[0][0] == player && board[1][1] == player && board[2][2] == player) ||
        (board[0][2] == player && board[1][1] == player && board[2][0] == player))
        return true;
    return false;
}

// Function to check if the game is a draw
bool isDraw() {
    for (int i = 0; i < 3; i++)
        for (int j = 0; j < 3; j++)
            if (board[i][j] != 'X' && board[i][j] != 'O')
                return false; // If any cell is empty, it's not a draw
    return true; // All cells are filled
}

// Function for player's move
void playerMove() {
    int choice;
    cout << "Enter your move (1-9): ";
    cin >> choice;
    // Validate the input
    while (choice < 1 || choice > 9 || board[(choice - 1) / 3][(choice - 1) % 3] == 'X' || board[(choice - 1) / 3][(choice - 1) % 3] == 'O') {
        cout << "Invalid move. Try again: ";
        cin >> choice;
    }
    // Mark the board with player's move
    board[(choice - 1) / 3][(choice - 1) % 3] = 'X';
}

// Minimax algorithm to calculate the best move for AI
int minimax(bool isMaximizing) {
    // Base cases for winning or drawing
    if (checkWin('X')) return -10; // Player wins
    if (checkWin('O')) return 10;  // AI wins
    if (isDraw()) return 0;        // It's a draw

    if (isMaximizing) {
        int bestScore = -1000; // Start with the worst score for maximizing player
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                // Check for empty cells
                if (board[i][j] != 'X' && board[i][j] != 'O') {
                    board[i][j] = 'O'; // Make a move for AI
                    int score = minimax(false); // Recur for minimizing player
                    board[i][j] = char(i * 3 + j + '1'); // Reset the cell
                    bestScore = max(score, bestScore); // Update best score
                }
            }
        }
        return bestScore; // Return the best score for AI
    } else {
        int bestScore = 1000; // Start with the worst score for minimizing player
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                // Check for empty cells
                if (board[i][j] != 'X' && board[i][j] != 'O') {
                    board[i][j] = 'X'; // Make a move for player
                    int score = minimax(true); // Recur for maximizing player
                    board[i][j] = char(i * 3 + j + '1'); // Reset the cell
                    bestScore = min(score, bestScore); // Update best score
                }
            }
        }
        return bestScore; // Return the best score for player
    }
}

// Function for AI's move
void aiMove() {
    int bestScore = -1000; // Best score for AI
    int move; // Variable to store the best move
    // Iterate through the board to find the best move
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            // Check for empty cells
            if (board[i][j] != 'X' && board[i][j] != 'O') {
                board[i][j] = 'O'; // Make a move for AI
                int score = minimax(false); // Get the score for this move
                board[i][j] = char(i * 3 + j + '1'); // Reset the cell
                if (score > bestScore) { // Update best move if score is better
                    bestScore = score; // Update best score
                    move = i * 3 + j + 1; // Store the position of the best move
                }
            }
        }
    }
    // Make the best move on the board
    board[(move - 1) / 3][(move - 1) % 3] = 'O';
}

// Main function to run the game
int main() {
    while (true) {
        displayBoard(); // Show the current board
        playerMove(); // Player makes a move
        // Check for win conditions
        if (checkWin('X')) {
            displayBoard();
            cout << "You win!\n"; // Player wins
            break;
        }
        if (isDraw()) {
            displayBoard();
            cout << "It's a draw!\n"; // It's a draw
            break;
        }
        aiMove(); // AI makes a move
        // Check for win conditions
        if (checkWin('O')) {
            displayBoard();
            cout << "AI wins!\n"; // AI wins
            break;
        }
        if (isDraw()) {
            displayBoard();
            cout << "It's a draw!\n"; // It's a draw
            break;
        }
    }
    return 0; // End of the program
}
   g++ main.cpp -o tictactoe

2. Run the game:
   ./tictactoe
   
Example:
Current board:
 1 | 2 | 3
---|---|---
 4 | 5 | 6
---|---|---
 7 | 8 | 9
 
Enter your move (1-9): 5
...
AI wins!
