# Part 1: Display Heading and Menu
def display_menu():
    print("\nCountry Information System")
    print("1. View all countries")
    print("2. Add a country")
    print("3. Delete a country")
    print("4. Exit\n")

# Part 2: Prepopulate the dictionary with at least 3 countries
def prepopulate_countries():
    return {
        'USA': 'Population: 331M, Capital: Washington D.C.',
        'Canada': 'Population: 37M, Capital: Ottawa',
        'Mexico': 'Population: 128M, Capital: Mexico City'
    }

# Part 3: View a Country (Loop through and display keys, prompt user for input)
def view_countries(countries):
    print("\nCountries available: ")
    for country in countries:
        print(country)
    
    country = input("Enter the name of the country to view: ").capitalize()
    if country in countries:
        print(f"{country}: {countries[country]}")
    else:
        print("Country not found.")

# Add a country (Check if it already exists)
def add_country(countries):
    country = input("Enter the name of the country to add: ").capitalize()
    if country in countries:
        print("Country already exists.")
    else:
        info = input(f"Enter the data for {country}: ")
        countries[country] = info
        print(f"{country} added successfully.")

# Delete a country (Remove a country from the dictionary)
def delete_country(countries):
    country = input("Enter the name of the country to delete: ").capitalize()
    if country in countries:
        del countries[country]
        print(f"{country} deleted successfully.")
    else:
        print("Country not found.")

# Main program loop
def main():
    countries = prepopulate_countries()  # Prepopulate with 3 countries
    
    while True:
        display_menu()  # Show the menu options
        choice = input("Enter your choice (1-4): ")

        if choice == '1':  # View all countries
            view_countries(countries)
        elif choice == '2':  # Add a country
            add_country(countries)
        elif choice == '3':  # Delete a country
            delete_country(countries)
        elif choice == '4':  # Exit the program
            print("Exiting the program.")
            break
        else:
            print("Invalid choice. Please try again.")  # Handle invalid input

if _name_ == "_main_":
    main()  # Run the program
