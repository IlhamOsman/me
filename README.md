def rectangle_calculator():
    while True:
        print("Rectangle Calculator")
        
        # Get height and width from the user
        try:
            height = int(input("Height: "))
            width = int(input("Width: "))
        except ValueError:
            print("Please enter valid integers for height and width.")
            continue
        
        # Calculate perimeter and area
        perimeter = 2 * (height + width)
        area = height * width

        # Display results
        print(f"Perimeter: {perimeter}")
        print(f"Area: {area}")
        
        # Draw the rectangle using asterisks
        print("Rectangle:")
        for i in range(height):
            if i == 0 or i == height - 1:  # Top and bottom rows
                print("*" * width)
            else:  # Middle rows with spaces inside
                print("" + " " * (width - 2) + "")
        
        # Display the height using asterisks
        print("\nHeight visualization:")
        for _ in range(height):
            print("*")

        # Ask user if they want to continue
        cont = input("Continue? (y/n): ").lower()
        if cont != 'y':
            print("Goodbye!")
            break

# Run the rectangle calculator
rectangle_calculator()
