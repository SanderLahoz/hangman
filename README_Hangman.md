# hangman
a replica of the simple game hangman created using python and ASCII art.


# Imports the random module
import random

#Creates a list with all the ASCII art in it.
HANGMAN_PICS = ["""
    +---+
        |
        |
        |
       ===""", """
    +---+
    0   |
        |
        |
       ===""", """
    +---+
    0   |
    |   |
        |
       ===""", """
    +---+
    0   |
   /|   |
        |
       ===""", """
    +---+
    0   |
   /|\  |
        |
       ===""", """
    +---+
    0   |
   /|\  |
   /    |
       ===""", """
    +---+
    0   |
   /|\  |
   / \  |
       ==="""]


#Uses the .Slit() function to change a string to a list.

words = """ant baboob badger bat bear beaver camel cat clam cobra cougar coyote crow
deer dog donkey duck eagle ferret fox frog goat goose hawk lion lizard llama mole
monkey moose mouse mule newt otter owl panda parrot pigeon python rabbit ram rat raven
rhino salmon seal shark sheep skunk sloth snake spider stork swan tiger toad trout
turkey turtle weasel whale wolf zebra""".split()




#defines a function that gets the random word.

def getRandomWord(wordList):
    wordIndex = random.randint(0, len(wordList) -1)
    return wordList[wordIndex]

#defines a function that displays the appropriate board.

def displayBoard(missedLetters, correctLetters, secretWord):
    print(HANGMAN_PICS[len(missedLetters)])
    print()

    print("Missed letters:", end="")
    for letter in missedLetters:
        print(letter, end="")
    print()

    blanks = "_" * len(secretWord)

    for i in range(len(secretWord)):
        if secretWord[i] in correctLetters:
            blanks = blanks[:i] + secretWord[i] + blanks[i+1:]

    for letter in blanks:
        print(letter, end="")
    print()

#defines a function that checks the users guess.

def getGuess(alreadyGuessed):
    while True:
        print("Guess a letter")
        guess = input()
        guess = guess.lower()
        if len(guess) != 1:
            print("Please enter a single letter.")
        elif guess in alreadyGuessed:
            print("You have already guessed that letter. Choose again.")
        elif guess not in "abcdefghijklmnopqrstuvwxyz":
            print("Please enter a LETTER.")
        else:
            return guess

#defines the main function whicth runs the game.

def playAgain():
    print("Do you want to play again? (yes or no)")
    return input().lower().startswith("y")

print("H A N G M A N")
missedLetters = ""
correctLetters = ""
secretWord = getRandomWord(words)
gameIsDone = False

while True:
    displayBoard(missedLetters, correctLetters, secretWord)
    
    guess = getGuess(missedLetters + correctLetters)
    
    if guess in secretWord:
        correctLetters = correctLetters + guess
        
        foundAllLetters = True
        for i in range(len(secretWord)):
            if secretWord[i] not in correctLetters:
                foundAllLetters = False
                break
        if foundAllLetters:
            print("Yes! The secret word is " + secretWord + "! you have won!")
            gameIsDone = True

    else:
        missedLetters = missedLetters + guess

        if len(missedLetters) == len(HANGMAN_PICS) - 1:
            displayBoard(missedLetters, correctLetters, secretWord)
            print("You have run out of guesses!\nAfter " + str(len(missedLetters)) +
                  " missed guesses and " + str(len(correctLetters)) + " correct guesses, the word was " +
                  secretWord + '"')
            gameIsDone = True

    if gameIsDone:
        if playAgain():
            missedLetters = ""
            correctLetters = ""
            gameIsDone = False
            secretWord = getRandomWord(words)
        else:
            break
