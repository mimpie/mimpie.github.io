```python
import random
def getPlayerLetter():
    while True: # 반복
        letter = input('Do you want be X or O? ').upper() # 문자 입력
        if letter == 'X': return 'X', 'O' # 'X'일 때, ('X', 'O') 반환
        elif letter == 'O': return 'O', 'X' # 'O'일 때, ('O', 'X') 반환
def getFirst():
    isCom1st = random.randint(0, 1) # 선 결정(랜덤)
    if isCom1st: print('The computer will go first.') # 선 결정 결과 출력(Computer 선)
    else: print('The player will go first.') # 선 결정 결과 출력(Player 선)
    return isCom1st # 결정값 반환

def drawBoard(board): # board: 크기 10의 리스트
    print('+---+---+---+')
    print('| '+board[7]+' | '+board[8]+' | '+board[9]+' |')
    print('+---+---+---+')
    print('| '+board[4]+' | '+board[5]+' | '+board[6]+' |')
    print('+---+---+---+')
    print('| '+board[1]+' | '+board[2]+' | '+board[3]+' |')
    print('+---+---+---+')
def isSpaceFree(board, move):
    return board[move] == ' ' 
def getComMove(board):
    for move in [5, 7, 9, 1, 3, 8, 4, 2, 6]: # 중앙(5), 꼭지점(1, 3, 7, 9), 나머지(2, 4, 6, 8) 순으로
        if isSpaceFree(board, move): # 해당 위치가 비어있는 경우
            return move # 해당 값 반환

def getPlayerMove(board):
    while True: # 반복
        move = input('What is your next move? (1-9) ') # 입력
        if move.isdigit(): # 숫자인지 확인
            move = int(move) # 숫자로 변환
            if move > 0 and move < 10 and isSpaceFree(board, move): # 1~9이고 빈 자리일 때
                return move # move 반환
def makeMove(board, letter, move): board[move] = letter
def isSameLetter(board, line):
    return board[line[0]] == board[line[1]] and board[line[1]] == board[line[2]]
def isWin(board, move):
    for line in [(1, 2, 3), (4, 5, 6), (7, 8, 9), (1, 4, 7), (2, 5, 8), (3, 6, 9), (3, 5, 7), (1, 5, 9)]:
        if move in line and isSameLetter(board, line) : return True
    return False

def undoMove(board, move):
    board[move] = ' ' 

def getWinMove(board, letter):
    for move in range(1, 10): # move 1~9 위치 확인
        if isSpaceFree(board, move): # 비어 있는 경우
            makeMove(board, letter, move) # 해당 위치에 letter를 배치
            winResult = isWin(board, move) # 승리확인(winResult에 결과 저장)
            undoMove(board, move) # letter 배치 취소
            if winResult: return move # 승리한 경우 해당 위치 반환
    return 0 # 모든 move(1~10)에 대해 승리 위치가 없는 경우

def getComMove(board, letters):
    for i in [1,0]:
        move = getWinMove(board, letters[i]) # Computer, player 순 승리할 수 있는 move 계산
        if move: return move # move가 존재할 경우 반환
    for i in [5, 7, 9, 1, 3, 8, 4, 2, 6]: # 중앙(5), 꼭지점(1, 3, 7, 9), 나머지 순
        if isSpaceFree(board, i): return i # 해당 위치가 비어있는 경우 해당 값 반환
        
        
        
letters = getPlayerLetter() # 문자 선택(0: player, 1: computer)
isCom1st = getFirst() # 선 결정
board = [' '] * 10 # 보드 초기화
for isComTurn in [isCom1st, 1-isCom1st] * 4 + [isCom1st]: # 9번의 move 동안 반복
    if isComTurn: move = getComMove(board, letters) # Computer turn일 때 move 결정
    else: drawBoard(board); move = getPlayerMove(board) # Player turn일 때 move 결정
    makeMove(board, letters[isComTurn], move) # 보드에 말 추가
    
    if isWin(board, move): # 승리 판단
        if isComTurn: print('You lose.') # computer 순서였을 때
        else: print('You have won the game!') # player 순서였을 때
        break
else: print('The game is a tie!') # 비김
drawBoard(board) #   마지막 보드 출력
```

    Do you want be X or O? O
    The player will go first.
    +---+---+---+
    |   |   |   |
    +---+---+---+
    |   |   |   |
    +---+---+---+
    |   |   |   |
    +---+---+---+
    What is your next move? (1-9) 3
    +---+---+---+
    |   |   |   |
    +---+---+---+
    |   | X |   |
    +---+---+---+
    |   |   | O |
    +---+---+---+
    What is your next move? (1-9) 6
    +---+---+---+
    |   |   | X |
    +---+---+---+
    |   | X | O |
    +---+---+---+
    |   |   | O |
    +---+---+---+
    What is your next move? (1-9) 2
    You lose.
    +---+---+---+
    |   |   | X |
    +---+---+---+
    |   | X | O |
    +---+---+---+
    | X | O | O |
    +---+---+---+
    




```python

```
