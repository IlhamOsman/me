from datetime import datetime

# Payroll Management System Functions
def get_dates():
    """Prompts user for 'from' and 'to' dates with validation."""
    while True:
        try:
            from_date = input("Enter from date (mm/dd/yyyy): ")
            if from_date.lower() == "all":
                return "All", None
            datetime.strptime(from_date, "%m/%d/%Y")  # Validate date format
            to_date = input("Enter to date (mm/dd/yyyy): ")
            datetime.strptime(to_date, "%m/%d/%Y")
            return from_date, to_date
        except ValueError:
            print("Invalid date format. Please enter date in mm/dd/yyyy format.")

def calculate_pay(hours, rate, tax_rate):
    """Calculates gross pay, tax, and net pay based on hours, rate, and tax rate."""
    gross = hours * rate
    tax = gross * tax_rate
    return gross, tax, gross - tax

def display_employee_info(from_date, to_date, name, hours, rate, gross, tax_rate, tax, net):
    """Displays details for a single employee."""
    print(f"\nFrom Date: {from_date} To Date: {to_date}")
    print(f"Employee Name: {name}")
    print(f"Hours Worked: {hours}, Hourly Rate: ${rate:.2f}")
    print(f"Gross Pay: ${gross:.2f}, Income Tax Rate: {tax_rate * 100:.1f}%")
    print(f"Income Tax: ${tax:.2f}, Net Pay: ${net:.2f}\n")

def write_to_file(employee):
    """Writes employee data to a text file in a pipe-delimited format."""
    with open("employee_data.txt", "a") as file:
        file.write(f"{employee['from_date']}|{employee['to_date']}|{employee['name']}|{employee['hours']}|"
                   f"{employee['rate']}|{employee['gross']}|{employee['tax_rate']}|{employee['tax']}|{employee['net']}\n")

# Admin Menu
def admin_menu(user_list):
    """Displays the admin menu for user and payroll management."""
    while True:
        print("\n--- Admin Menu ---")
        print("1. Add New User")
        print("2. Display All Users")
        print("3. Manage Payroll")
        print("4. Logout")
        choice = input("Enter your choice: ").strip()

        if choice == "1":
            validate_and_store_user_data("user_data.txt", user_list)
        elif choice == "2":
            display_all_user_data(user_list)
        elif choice == "3":
            manage_payroll()
        elif choice == "4":
            print("Logging out...")
            break
        else:
            print("Invalid choice. Please try again.")

# User Menu
def display_user_data(user):
    """Displays the details of a single user."""
    print("\n--- User Details ---")
    print(f"User ID: {user.user_id}")
    print(f"Password: {user.password}")
    print(f"Authorization Code: {user.authorization}")

# Login System Classes and Functions
class Login:
    """Represents a user login with user ID, password, and authorization."""
    def _init_(self, user_id, password, authorization):
        self.user_id = user_id
        self.password = password
        self.authorization = authorization

def read_user_data(filename):
    """Reads user data from a file and returns a list of Login objects."""
    user_list = []
    try:
        with open(filename, "r") as file:
            for line in file:
                user_id, password, authorization = line.strip().split("|")
                user_list.append(Login(user_id, password, authorization))
    except FileNotFoundError:
        print("No user data found. Please ensure the file exists.")
    return user_list

def validate_and_store_user_data(filename, user_list):
    """Collects and validates user data, then writes it to the file."""
    while True:
        user_id = input("Enter User ID (or type 'End' to stop): ").strip()
        if user_id.lower() == "end":
            break

        if any(user.user_id == user_id for user in user_list):
            print("Error: User ID already exists. Please try again.")
            continue

        password = input("Enter Password: ").strip()
        authorization_code = input("Enter Authorization Code (Admin/User): ").strip()

        if authorization_code not in ["Admin", "User"]:
            print("Error: Invalid Authorization Code. Please enter 'Admin' or 'User'.")
            continue

        new_user = Login(user_id, password, authorization_code)
        user_list.append(new_user)

        with open(filename, "a") as file:
            file.write(f"{user_id}|{password}|{authorization_code}\n")

        print("User data successfully added!\n")

# Updated login process for Admin/User roles
def login_process(user_list):
    """Handles the login process and grants access based on authorization."""
    user_id = input("Enter User ID: ").strip()
    password = input("Enter Password: ").strip()

    for user in user_list:
        if user.user_id == user_id:
            if user.password == password:
                print("\nLogin Successful!")
                if user.authorization == "Admin":
                    admin_menu(user_list)
                elif user.authorization == "User":
                    display_user_data(user)
                return
            else:
                print("\nError: Incorrect Password.")
                return
    print("\nError: User ID not found.")

# Main function to start the program
def main():
    user_filename = "user_data.txt"
    user_list = read_user_data(user_filename)

    print("\n--- User Login ---")
    login_process(user_list)

if _name_ == "_main_":
    main()
