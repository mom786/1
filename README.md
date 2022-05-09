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
