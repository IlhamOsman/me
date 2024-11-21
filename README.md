# Rectangle Calculator Program
while True:
    print("\nRectangle Calculator")
    
    # Input height and width from user
    try:
        height = int(input("Height: "))
        width = int(input("Width: "))
    except ValueError:
        print("Invalid input! Please enter valid integers for height and width.")
        continue

    # Calculate the perimeter and area
    perimeter = 2 * (height + width)
    area = height * width

    # Display the results
    print(f"Perimeter: {perimeter}")
    print(f"Area: {area}")

    # Print the rectangle border
    print("Rectangle:")
    for row in range(height):
        if row == 0 or row == height - 1:  # Top and bottom border
            print("* " * width)
        else:  # Middle rows with spaces
            print("* " + "  " * (width - 2) + "* ")

    # Prompt to continue or exit
    continue_choice = input("Continue? (y/n): ").strip().lower()
    if continue_choice != 'y':
        print("Goodbye!")
        break
