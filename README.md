import random

def create_deck():
    """Create a standard 52-card deck."""
    suits = ['Hearts', 'Diamonds', 'Clubs', 'Spades']
    ranks = ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'Jack', 'Queen', 'King', 'Ace']
    return [f"{rank} of {suit}" for suit in suits for rank in ranks]

def main():
    # Create and shuffle the deck
    deck = create_deck()
    random.shuffle(deck)
    print("Card Dealer")
    print("I have shuffled a deck of 52 cards.")
    
    while True:
        try:
            num_cards = int(input("How many cards would you like? "))
            if num_cards < 1 or num_cards > len(deck):
                print(f"Please enter a number between 1 and {len(deck)}.")
                continue
        except ValueError:
            print("Please enter a valid number.")
            continue

        # Deal the cards
        dealt_cards = [deck.pop() for _ in range(num_cards)]
        print("\nHere are your cards:")
        for card in dealt_cards:
            print(card)

        print(f"\nThere are {len(deck)} cards left in the deck.")
        
        # Check if the deck is empty
        if not deck:
            print("The deck is empty! Thanks for playing.")
            break

        # Ask if the user wants to continue
        cont = input("\nGood luck! Do you want to draw more cards? (yes/no): ").strip().lower()
        if cont != 'yes':
            print("Thanks for playing!")
            break

if _name_ == "_main_":
    main()
