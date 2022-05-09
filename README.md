<!-- **********************************************************DFS**********************************************************
#A class to represent a graph object
class Graph:
    # Constructor
    def __init__(self, edges, n):
        # A list of lists to represent an adjacency list
        self.adjList = [[] for _ in range(n)]
 
        # add edges to the undirected graph
        for (src, dest) in edges:
            self.adjList[src].append(dest)
            self.adjList[dest].append(src)
 
 
#Function to perform DFS traversal on the graph on a graph
def DFS(graph, v, discovered):
 
    discovered[v] = True            # mark the current node as discovered
    print(v, end=' ')               # print the current node
 
    # do for every edge (v, u)
    for u in graph.adjList[v]:
        if not discovered[u]:       # if `u` is not yet discovered
            DFS(graph, u, discovered)
 
 
if __name__ == '__main__':
 
    # List of graph edges as per the above diagram
    edges = [
        # Notice that node 0 is unconnected
        (1, 2), (1, 7), (1, 8), (2, 3), (2, 6), (3, 4),
        (3, 5), (8, 9), (8, 12), (9, 10), (9, 11)
    ]
 
    # total number of nodes in the graph (labelled from 0 to 12)
    n = 13
 
    # build a graph from the given edges
    graph = Graph(edges, n)
 
    # to keep track of whether a vertex is discovered or not
    discovered = [False] * n
 
    # Perform DFS traversal from all undiscovered nodes to
    # cover all connected components of a graph
    for i in range(n):
        if not discovered[i]:
            DFS(graph, i, discovered)

**********************************************************BFS**********************************************************

from collections import deque
 
 
#A class to represent a graph object
class Graph:
    # Constructor
    def __init__(self, edges, n):
 
        # A list of lists to represent an adjacency list
        self.adjList = [[] for _ in range(n)]
 
        # add edges to the undirected graph
        for (src, dest) in edges:
            self.adjList[src].append(dest)
            self.adjList[dest].append(src)
 
 
#Perform BFS on the graph starting from vertex `v`
def BFS(graph, v, discovered):
 
    # create a queue for doing BFS
    q = deque()
 
    # mark the source vertex as discovered
    discovered[v] = True
 
    # enqueue source vertex
    q.append(v)
 
    # loop till queue is empty
    while q:
 
        # dequeue front node and print it
        v = q.popleft()
        print(v, end=' ')
 
        # do for every edge (v, u)
        for u in graph.adjList[v]:
            if not discovered[u]:
                # mark it as discovered and enqueue it
                discovered[u] = True
                q.append(u)
 
 
if __name__ == '__main__':
 
    # List of graph edges as per the above diagram
    edges = [
        (1, 2), (1, 3), (1, 4), (2, 5), (2, 6), (5, 9),
        (5, 10), (4, 7), (4, 8), (7, 11), (7, 12)
        # vertex 0, 13, and 14 are single nodes
    ]
 
    # total number of nodes in the graph (labelled from 0 to 14)
    n = 15
 
    # build a graph from the given edges
    graph = Graph(edges, n)
 
    # to keep track of whether a vertex is discovered or not
    discovered = [False] * n
 
    # Perform BFS traversal from all undiscovered nodes to
    # cover all connected components of a graph
    for i in range(n):
        if not discovered[i]:
            # start BFS traversal from vertex i
            BFS(graph, i, discovered)
**********************************************************Puzzle**********************************************************
class Node:
    def __init__(self, data, level, fval):
        # Initialize the node with the data ,level of the node and the calculated fvalue
        self.data = data
        self.level = level
        self.fval = fval

    def generate_child(self):
        # Generate child nodes from the given node by moving the blank space
        # either in the four direction {up,down,left,right}
        x, y = self.find(self.data, '_')
        # val_list contains position values for moving the blank space in either of
        # the 4 direction [up,down,left,right] respectively.
        val_list = [[x, y - 1], [x, y + 1], [x - 1, y], [x + 1, y]]
        children = []
        for i in val_list:
            child = self.shuffle(self.data, x, y, i[0], i[1])
            if child is not None:
                child_node = Node(child, self.level + 1, 0)
                children.append(child_node)
        return children

    def shuffle(self, puz, x1, y1, x2, y2):
        # Move the blank space in the given direction and if the position value are out
        # of limits the return None
        if x2 >= 0 and x2 < len(self.data) and y2 >= 0 and y2 < len(self.data):
            temp_puz = []
            temp_puz = self.copy(puz)
            temp = temp_puz[x2][y2]
            temp_puz[x2][y2] = temp_puz[x1][y1]
            temp_puz[x1][y1] = temp
            return temp_puz
        else:
            return None

    def copy(self, root):
        # copy function to create a similar matrix of the given node
        temp = []
        for i in root:
            t = []
            for j in i:
                t.append(j)
            temp.append(t)
        return temp

    def find(self, puz, x):
        # Specifically used to find the position of the blank space
        for i in range(0, len(self.data)):
            for j in range(0, len(self.data)):
                if puz[i][j] == x:
                    return i, j


class Puzzle:
    def __init__(self, size):
        # Initialize the puzzle size by the the specified size,open and closed lists to empty
        self.n = size
        self.open = []
        self.closed = []

    def accept(self):
        # Accepts the puzzle from the user
        puz = []
        for i in range(0, self.n):
            temp = input().split(" ")
            puz.append(temp)
        return puz

    def f(self, start, goal):
        # Heuristic function to calculate Heuristic value f(x) = h(x) + g(x)
        return self.h(start.data, goal) + start.level

    def h(self, start, goal):
        # Calculates the difference between the given puzzles
        temp = 0
        for i in range(0, self.n):
            for j in range(0, self.n):
                if start[i][j] != goal[i][j] and start[i][j] != '_':
                    temp += 1
        return temp

    def process(self):
        # Accept Start and Goal Puzzle state
        print("enter the start state matrix \n")
        start = self.accept()
        print("enter the goal state matrix \n")
        goal = self.accept()
        start = Node(start, 0, 0)
        start.fval = self.f(start, goal)
        # put the start node in the open list
        self.open.append(start)
        print("\n\n")
        while True:
            cur = self.open[0]
            print("==================================================\n")
            for i in cur.data:
                for j in i:
                    print(j, end=" ")
                print("")
            # if the difference between current and goal node is 0 we have reached the goal node
            if (self.h(cur.data, goal) == 0):
                break
            for i in cur.generate_child():
                i.fval = self.f(i, goal)
                self.open.append(i)
            self.closed.append(cur)
            del self.open[0]
            # sort the open list based on f value
            self.open.sort(key=lambda x: x.fval, reverse=False)


puz = Puzzle(3)
puz.process()

**********************************************************greedy selection sort**********************************************************
import sys
A = [64, 25, 12, 22, 11]
  
#Traverse through all array elements
for i in range(len(A)):
      
    # Find the minimum element in remaining 
    # unsorted array
    min_idx = i
    for j in range(i+1, len(A)):
        if A[min_idx] > A[j]:
            min_idx = j
              
    # Swap the found minimum element with 
    # the first element        
    A[i], A[min_idx] = A[min_idx], A[i]
  
#Driver code to test above
print ("Sorted array")
for i in range(len(A)):
    print("%d" %A[i],end=" ") 

*************************************************n queen problem********************************************************
global N
N = 4

def isSafe(board, row, col):
    for i in range(col):
        if board[row][i] == 1:
            return False

    for i, j in zip(range(row, -1, -1), range(col, -1, -1)):
         if board[i][j] == 1:
                return False

    for i, j in zip(range(row, N, -1), range(col, -1, -1)):
         if board[i][j] == 1:
                return False

    return True

def solveNQUtil(board, col):
    if col >= N:
        return True
    for i in range(N):
        if isSafe(board, i, col):
            board[i][col] = 1

            if solveNQUtil(board, col +1):
                return True
            
            board[i][col] = 0

    return False

def printSolution(board):
    for i in range(N):
        for j in range(N):
            print(board[i][j], end = " ")
        print(" ")


#def solveNQ():
board = [[0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0]]
if solveNQUtil(board, 0) == False:
    print("Solution doesn't exist")
    # return False
printSolution(board)
    # return True

#solveNQ()

*************************************************chatbot********************************************************
import nltk
from nltk.chat.util import Chat, reflections

reflections = {
    "i am": "you are",
    "i was": "you were",
    "i": "you",
    "i'm": "you are",
    "i'd": "you would",
    "i've": "you have",
    "i'll": "you will",
    "my": "your",
    "you are": "I am",
    "you were": "I was",
    "you've": "I have",
    "you'll": "I will",
    "your": "my",
    "yours": "mine",
    "you": "me",
    "me": "you"
}

pairs = [
    [
        r"my name is (.*)",
        ["Hello %1, How are you today ?", ]
    ],
    [
        r"hi|hey|hello",
        ["Hello", "Hey there", ]
    ],
    [
        r"what is your name ?",
        ["I am a bot created by Pradnya. You can call me crazy!", ]
    ],
    [
        r"how are you ?",
        ["I'm doing good\nHow about You ?", ]
    ],
    [
        r"sorry (.*)",
        ["Its alright", "Its OK, never mind", ]
    ],
    [
        r"I am fine",
        ["Great to hear that, How can I help you?", ]
    ],
    [
        r"i'm (.*) doing good",
        ["Nice to hear that", "How can I help you?:)", ]
    ],
    [
        r"(.*) age?",
        ["I'm a computer program dude. Seriously you are asking me this?", ]
    ],
    [
        r"what (.*) want ?",
        ["Make me an offer I can't refuse", ]
    ],
    [
        r"(.*) created ?",
        ["Pradnya created me using Python's NLTK library ", "top secret ;)", ]
    ],
    [
        r"(.*) (location|city) ?",
        ['Pune, Maharashtra', ]
    ],
    [
        r"how is weather in (.*)?",
        ["Weather in %1 is awesome like always", "Too hot man here in %1", "Too cold man here in %1",
         "Never even heard about %1"]
    ],
    [
        r"i work in (.*)?",
        ["%1 is an Amazing company, I have heard about it. But they are in huge loss these days.", ]
    ],
    [
        r"(.*)raining in (.*)",
        ["No rain since last week here in %2", "Damn its raining too much here in %2"]
    ],
    [
        r"how (.*) health(.*)",
        ["I'm a computer program, so I'm always healthy ", ]
    ],
    [
        r"(.*) (sports|game) ?",
        ["I'm a very big fan of Football", ]
    ],
    [
        r"who (.*) sportsperson ?",
        ["Messy", "Ronaldo", "Neymar"]
    ],
    [
        r"who (.*) (movie star|actor)?",
        ["Robert Downey Jr"]
    ],
    [
        r"quit",
        ["BBye take care. See you soon :) ", "It was nice talking to you. See you soon :)"]
    ],
]


def chat():
    print("Hi! I am a chat-bot created by Priyanka for your service")
    chat = Chat(pairs, reflections)
    chat.converse()


#initiate the conversation
if __name__ == "__main__":
    chat() -->
