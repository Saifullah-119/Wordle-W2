import random
from pymongo import MongoClient

# MongoDB setup
client = MongoClient('placeholder')  //put your client in placeholder
db = client['wordleDB']
collection = db['words']

# Game logic
def fetch_word(word_length):
    # Fetch a random word of the desired length from the MongoDB
    word_data = collection.find_one({"length": word_length})
    word_list = word_data["words"]
    return random.choice([word.lower() for word in word_list])

def validate_input(word, word_length):
    # Ensure input is of correct length and alphabetical
    if len(word) != word_length or not word.isalpha():
        return False
    return True

def give_feedback(guess, word):
    feedback = []
    for i in range(len(guess)):
        if guess[i] == word[i]:
            feedback.append("+")  # Right letter, right position
        elif guess[i] in word:
            feedback.append("_")  # Right letter, wrong position
        else:
            feedback.append("x")  # Wrong letter
    return ''.join(feedback)

def wordle_game():
    while True:
        try:
            word_length = int(input("Choose word length (5, 6, or 7): "))
            if word_length not in [5, 6, 7]:
                print("Please choose a valid word length.")
                continue
        except ValueError:
            print("Please enter a number.")
            continue
        break

    secret_word = fetch_word(word_length)
    attempts = 6

    print("\nWordle Guide:")
    print("x = wrong letter")
    print("_ = right letter, wrong position")
    print("+ = right letter, right position\n")

    for attempt in range(1, attempts + 1):
        while True:
            guess = input(f"Attempt {attempt}/{attempts} - Enter your guess: ").lower()
            if validate_input(guess, word_length):
                break
            print(f"Please enter a valid {word_length}-letter word.")

        feedback = give_feedback(guess, secret_word)
        print(f"Feedback: {feedback}\n")

        if feedback == "+" * word_length:
            print(f"Congratulations! You've guessed the correct word '{secret_word}' in {attempt} attempts!")
            break
    else:
        print(f"Sorry, you've used all attempts. The correct word was: '{secret_word}'")

    restart = input("Do you want to play again? (y/n): ").lower()
    if restart == 'y':
        wordle_game()
    else:
        print("Thanks for playing! Goodbye!")

if __name__ == "__main__":
    wordle_game()
