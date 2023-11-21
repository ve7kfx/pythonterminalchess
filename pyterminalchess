import chess
from stockfish import Stockfish
import sys

# Set the path to your Stockfish engine
stockfish_path = '/usr/games/stockfish'  # Update this path as per your Stockfish installation
stockfish = Stockfish(stockfish_path)

# Function to set Stockfish's level
def set_stockfish_level(level):
    stockfish.set_skill_level(level)

def print_board(board):
    # Symbols for white pieces (outlined)
    white_piece_symbols = {
        chess.PAWN: '♙', chess.KNIGHT: '♘', chess.BISHOP: '♗',
        chess.ROOK: '♖', chess.QUEEN: '♕', chess.KING: '♔',
    }

    # Symbols for black pieces (filled)
    black_piece_symbols = {
        chess.PAWN: '♟', chess.KNIGHT: '♞', chess.BISHOP: '♝',
        chess.ROOK: '♜', chess.QUEEN: '♛', chess.KING: '♚',
    }

    # Print top border
    print("   +" + "---+" * 8)

    for rank in range(7, -1, -1):
        print(f'{rank + 1} |', end='')  # Print rank number with left border
        for file in range(8):
            piece = board.piece_at(chess.square(file, rank))
            if piece:
                symbol = white_piece_symbols[piece.piece_type] if piece.color == chess.WHITE else black_piece_symbols[piece.piece_type]
            else:
                symbol = ' '  # Empty square
            print(f' {symbol} |', end='')  # Each square is contained within vertical lines
        print("\n   +" + "---+" * 8)  # Print horizontal line after each rank

    # Print file letters below the board
    print('     a   b   c   d   e   f   g   h')

def get_user_move(board):
    move = input("Enter your move (in UCI format, e.g., e2e4), or 'Q' to quit: ")
    if move.lower() == 'q':
        print("Quitting game.")
        sys.exit(0)

    try:
        chess_move = chess.Move.from_uci(move)
        if chess_move in board.legal_moves:
            return chess_move
        else:
            print("Illegal move.")
            return None
    except:
        print("Invalid move format.")
        return None

def main():
    level = int(input("Set Stockfish level (0-20): "))
    set_stockfish_level(level)
    player_color = input("Choose your color (White/Black): ").strip().lower()
    is_player_white = player_color != "black"
    board = chess.Board()

    last_move = None  # Variable to keep track of the last move made

    while not board.is_game_over():
        if (board.turn == chess.WHITE) == is_player_white:
            print_board(board)
            user_move = get_user_move(board)
            if user_move:
                board.push(user_move)
                last_move = user_move  # Update last move
        else:
            stockfish.set_position([move.uci() for move in board.move_stack])
            stockfish_move = stockfish.get_best_move()
            if stockfish_move:
                board.push(chess.Move.from_uci(stockfish_move))
                last_move = chess.Move.from_uci(stockfish_move)  # Update last move

        if last_move:
            print(f"Last move: {last_move.uci()}")  # Print the last move after each turn

    print_board(board)
    print("Game over.")

if __name__ == "__main__":
    main()
