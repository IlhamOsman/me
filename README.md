import random
#if _name_ == "_main_":

def main():
    print("Guess the number!")
    while True:
        play_game()  # Call the function to play the game
        play_again = input("Would you like to play again? (y/n): ").lower()
        if play_again != 'y':
            print("Goodbye!")
            break

def play_game():
    limit = int(input("Enter the limit: "))  # Prompt user for limit
    number_to_guess = random.randint(1, limit)  # Generate random number
    print(f"I'm thinking of a number from 1 to {limit}")
    
    attempts = 0
    while True:
        guess = int(input("Your guess: "))  # Prompt user for guess
        attempts += 1
        
        if guess < number_to_guess:
            print("Too low.")
        elif guess > number_to_guess:
            print("Too high.")
        else:
            print(f"You guessed it in {attempts} tries.")
            break

if _name_ == "_main_":
    main()
