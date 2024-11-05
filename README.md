mdef get_price():
    while True:
        try:
            price = float(input("Enter price: "))
            return price
        except ValueError:
            print("Invalid decimal number. Please try again.")

def get_quantity():
    while True:
        try:
            quantity = int(input("Enter quantity: "))
            return quantity
        except ValueError:
            print("Invalid integer. Please try again.")

def main():
    print("The Invoice Line Item Calculator")
    
    while True:
        # Get price and quantity
        price = get_price()
        quantity = get_quantity()
        
        # Calculate total
        total = price * quantity
        
        # Display results
        print(f"\nPRICE:\t\t{price:.2f}")
        print(f"QUANTITY:\t{quantity}")
        print(f"TOTAL:\t\t{total:.2f}\n")
        
        # Ask if user wants to enter another line item
        another = input("Enter another line item? (y/n): ").strip().lower()
        if another != 'y':
            break

    print("\nBye!")

# Run the program
main()
