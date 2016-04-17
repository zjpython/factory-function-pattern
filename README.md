# factory-function-pattern
工厂方法模式    
如果子类的某个方法要根据具体情况来决定 用什么类去实例化相关对象，那么可以考虑工厂方法模式。   
 - origin：   
```
class ChessBoard(AbstractBoard):

    def __init__(self):
        super().__init__(8, 8)


    def populate_board(self):
        self.board[0][0] = BlackChessRook()
        self.board[0][1] = BlackChessKnight()
        self.board[0][2] = BlackChessBishop()
        self.board[0][3] = BlackChessQueen()
        self.board[0][4] = BlackChessKing()
        self.board[0][5] = BlackChessBishop()
        self.board[0][6] = BlackChessKnight()
        self.board[0][7] = BlackChessRook()
        self.board[7][0] = WhiteChessRook()
        self.board[7][1] = WhiteChessKnight()
        self.board[7][2] = WhiteChessBishop()
        self.board[7][3] = WhiteChessQueen()
        self.board[7][4] = WhiteChessKing()
        self.board[7][5] = WhiteChessBishop()
        self.board[7][6] = WhiteChessKnight()
        self.board[7][7] = WhiteChessRook()
        for column in range(8):
            self.board[1][column] = BlackChessPawn()
```
 - Using factory function pattern   
```
class ChessBoard(AbstractBoard):

    def __init__(self):
        super().__init__(8, 8)


    def populate_board(self):
        for row, color in ((0, BLACK), (7, WHITE)):
            for columns, kind in (((0, 7), ROOK), ((1, 6), KNIGHT),
                    ((2, 5), BISHOP), ((3,), QUEEN), ((4,), KING)):
                for column in columns:
                    self.board[row][column] = create_piece(kind, color)
        for column in range(8):
            for row, color in ((1, BLACK), (6, WHITE)):
                self.board[row][column] = create_piece(PAWN, color)


def create_piece(kind, color):
    '''
    factory function
    '''
    color = "White" if color == WHITE else "Black"
    name = {DRAUGHT: "Draught", PAWN: "ChessPawn", ROOK: "ChessRook",
            KNIGHT: "ChessKnight", BISHOP: "ChessBishop",
            KING: "ChessKing", QUEEN: "ChessQueen"}[kind]
    return globals()[color + name]()
```
