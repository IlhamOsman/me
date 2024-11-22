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
        print("Login data file not found. Please create 'login_data.txt' with user credentials.")
    return login_list

# Function to handle the login process
def login_process(login_list):
    """Handles the login process with user validation."""
    user_id = input("Enter User ID: ").strip()
    password = input("Enter Password: ").strip()

    # Validate User ID and Password
    for login in login_list:
        if login.user_id == user_id:
            if login.password == password:
                print(f"\nWelcome, {user_id}!")
                print(f"Authorization Level: {login.authorization}\n")
                # Create a Login object and store properties
                current_user = Login(user_id, password, login.authorization)
                if current_user.authorization == "Admin":
                    print("Access granted: Admin privileges (can view and edit data).")
                    handle_admin_tasks()
                elif current_user.authorization == "User":
                    print("Access granted: User privileges (can view data only).")
                    handle_user_tasks()
                return
            else:
                print("Invalid password. Access denied.")
                return
    print("Invalid User ID. Access denied.")

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

# Function to read and display records
def read_and_display_records(filter_date):
    """Reads and displays records based on the given filter date."""
    try:
        with open("employee_data.txt", "r") as file:
            print("\n--- Employee Records ---")
            totals = {
                'employee_count': 0,
                'total_hours': 0,
                'total_gross': 0,
                'total_tax': 0,
                'total_net': 0
            }
            for line in file:
                record = line.strip().split("|")
                from_date = record[0]
                if filter_date == "All" or filter_date == from_date:
                    to_date, name, hours, rate, gross, tax_rate, tax, net = record[1:]
                    hours, rate, gross, tax, net, tax_rate = map(float, [hours, rate, gross, tax, net, tax_rate])
                    print(f"From Date: {from_date} To Date: {to_date}")
                    print(f"Employee Name: {name}")
                    print(f"Hours Worked: {hours}, Hourly Rate: ${rate:.2f}")
                    print(f"Gross Pay: ${gross:.2f}, Tax: ${tax:.2f}, Net Pay: ${net:.2f}\n")

                    totals['employee_count'] += 1
                    totals['total_hours'] += hours
                    totals['total_gross'] += gross
                    totals['total_tax'] += tax
                    totals['total_net'] += net

            print("\n--- Totals ---")
            print(f"Total Number of Employees: {totals['employee_count']}")
            print(f"Total Hours Worked: {totals['total_hours']}")
            print(f"Total Gross Pay: ${totals['total_gross']:.2f}")
            print(f"Total Tax: ${totals['total_tax']:.2f}")
            print(f"Total Net Pay: ${totals['total_net']:.2f}\n")
    except FileNotFoundError:
        print("No employee data found.")

# Main function
def main():
    # Load login data
    login_list = load_login_data("login_data.txt")

    # Perform login process
    if not login_list:
        return  # Exit if login data is not loaded
    login_process(login_list)

# Run the program
if _name_ == "_main_":
    main()
