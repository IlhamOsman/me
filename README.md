# Import necessary modules
from datetime import datetime

# Define the Login class
class Login:
    def _init_(self, user_id, password, authorization):
        self.user_id = user_id
        self.password = password
        self.authorization = authorization

# Function to read login data from a file
def load_login_data(file_name):
    """Loads login data from a file into a list of Login objects."""
    login_list = []
    try:
        with open(file_name, "r") as file:
            for line in file:
                user_id, password, authorization = line.strip().split("|")
                login_list.append(Login(user_id, password, authorization))
    except FileNotFoundError:
        print("Login data file not found.")
    return login_list

# Function to handle the login process
def login_process(login_list):
    """Handles the login process."""
    user_id = input("Enter User ID: ")
    password = input("Enter Password: ")

    for login in login_list:
        if login.user_id == user_id and login.password == password:
            print(f"Welcome, {user_id}!")
            if login.authorization == "Admin":
                print("Access granted: Admin privileges (can view and edit data).")
                handle_admin_tasks()
            elif login.authorization == "User":
                print("Access granted: User privileges (can view data only).")
                handle_user_tasks()
            return
    print("Invalid User ID or Password. Access denied.")

# Function to handle admin-specific tasks
def handle_admin_tasks():
    """Allows admin to view and edit data."""
    print("\nAdmin Menu:")
    print("1. View all records")
    print("2. Add a new record")
    choice = input("Enter your choice: ")

    if choice == "1":
        filter_date = input("\nEnter 'All' to display all records or a specific 'From Date' (mm/dd/yyyy): ")
        read_and_display_records(filter_date)
    elif choice == "2":
        main()  # Call the main function to add new records
    else:
        print("Invalid choice.")

# Function to handle user-specific tasks
def handle_user_tasks():
    """Allows users to view data only."""
    filter_date = input("\nEnter 'All' to display all records or a specific 'From Date' (mm/dd/yyyy): ")
    read_and_display_records(filter_date)

# Main function
def main():
    # Load login data
    login_list = load_login_data("login_data.txt")

    # Perform login process
    login_process(login_list)

    # Rest of the payroll processing logic as defined in your original code
    while True:
        if input("Enter 'End' to stop or press Enter to continue: ").strip().lower() == "end":
            break

        # Gather employee data
        from_date, to_date = get_dates()
        name = input("Enter employee name: ")
        try:
            hours = float(input("Enter hours worked: "))
            rate = float(input("Enter hourly rate: "))
            tax_rate = float(input("Enter tax rate (in %): ")) / 100
        except ValueError:
            print("Invalid numeric input. Please try again.")
            continue

        # Calculate pay details
        gross, tax, net = calculate_pay(hours, rate, tax_rate)

        # Display individual employee information
        display_employee_info(from_date, to_date, name, hours, rate, gross, tax_rate, tax, net)

        # Create and store employee data
        employee = {
            'from_date': from_date,
            'to_date': to_date,
            'name': name,
            'hours': hours,
            'rate': rate,
            'gross': gross,
            'tax_rate': tax_rate,
            'tax': tax,
            'net': net
        }
        write_to_file(employee)

    # Read and display records based on user input
    filter_date = input("\nEnter 'All' to display all records or a specific 'From Date' (mm/dd/yyyy): ")
    read_and_display_records(filter_date)

if _name_ == "_main_":
    main()
