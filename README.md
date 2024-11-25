import os
from datetime import datetime

# Function to authenticate user
def authenticate_user():
    """Authenticates a user by username and password."""
    try:
        with open("credentials.txt", "r") as file:
            credentials = [line.strip().split("|") for line in file]
    except FileNotFoundError:
        print("No credentials found. Please set up users.")
        return None, None

    username = input("Enter username: ")
    password = input("Enter password: ")

    for user in credentials:
        if username == user[0] and password == user[1]:
            return username, user[2]  # Return username and role (admin/employee)
    print("Invalid username or password.")
    return None, None


# Function to input and validate dates
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


# Function to calculate pay details
def calculate_pay(hours, rate, tax_rate):
    """Calculates gross pay, tax, and net pay based on hours, rate, and tax rate."""
    gross = hours * rate
    tax = gross * tax_rate
    return gross, tax, gross - tax


# Function to display individual employee information
def display_employee_info(from_date, to_date, name, hours, rate, gross, tax_rate, tax, net):
    """Displays details for a single employee."""
    print(f"\nFrom Date: {from_date} To Date: {to_date}")
    print(f"Employee Name: {name}")
    print(f"Hours Worked: {hours}, Hourly Rate: ${rate:.2f}")
    print(f"Gross Pay: ${gross:.2f}, Income Tax Rate: {tax_rate * 100:.1f}%")
    print(f"Income Tax: ${tax:.2f}, Net Pay: ${net:.2f}\n")


# Function to display totals
def display_totals(totals):
    """Displays cumulative totals for all employees processed."""
    print("\n--- Totals ---")
    print(f"Total Number of Employees: {totals['employee_count']}")
    print(f"Total Hours Worked: {totals['total_hours']}")
    print(f"Total Gross Pay: ${totals['total_gross']:.2f}")
    print(f"Total Income Tax: ${totals['total_tax']:.2f}")
    print(f"Total Net Pay: ${totals['total_net']:.2f}\n")


# Function to write employee data to a text file
def write_to_file(employee):
    """Writes employee data to a text file in a pipe-delimited format."""
    with open("employee_data.txt", "a") as file:
        file.write(f"{employee['from_date']}|{employee['to_date']}|{employee['name']}|{employee['hours']}|"
                   f"{employee['rate']}|{employee['gross']}|{employee['tax_rate']}|{employee['tax']}|{employee['net']}\n")


# Admin functions
def admin_menu():
    """Displays the admin menu."""
    while True:
        print("\n--- Admin Menu ---")
        print("1. Add Employee Payroll Data")
        print("2. View All Payroll Records")
        print("3. Generate Payroll Summary")
        print("4. Exit")

        choice = input("Enter your choice: ")
        if choice == "1":
            add_employee_data()
        elif choice == "2":
            read_and_display_records("All")
        elif choice == "3":
            generate_summary()
        elif choice == "4":
            break
        else:
            print("Invalid choice. Please try again.")


def add_employee_data():
    """Allows admin to add payroll data for an employee."""
    from_date, to_date = get_dates()
    name = input("Enter employee name: ")
    hours = float(input("Enter hours worked: "))
    rate = float(input("Enter hourly rate: "))
    tax_rate = float(input("Enter tax rate (in %): ")) / 100

    gross, tax, net = calculate_pay(hours, rate, tax_rate)

    # Create employee record
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
    print("Employee payroll data added successfully.")


def read_and_display_records(filter_date):
    """Displays all employee records filtered by a specific date."""
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
                    display_employee_info(from_date, to_date, name, hours, rate, gross, tax_rate, tax, net)

                    totals['employee_count'] += 1
                    totals['total_hours'] += hours
                    totals['total_gross'] += gross
                    totals['total_tax'] += tax
                    totals['total_net'] += net

            display_totals(totals)
    except FileNotFoundError:
        print("No employee data found.")


# Employee functions
def employee_menu(username):
    """Displays the employee menu."""
    while True:
        print(f"\n--- Employee Menu ({username}) ---")
        print("1. View My Payroll")
        print("2. Exit")

        choice = input("Enter your choice: ")
        if choice == "1":
            read_and_display_records_for_employee(username)
        elif choice == "2":
            break
        else:
            print("Invalid choice. Please try again.")


def read_and_display_records_for_employee(username):
    """Displays payroll records for the logged-in employee."""
    try:
        with open("employee_data.txt", "r") as file:
            print("\n--- Your Payroll Records ---")
            for line in file:
                record = line.strip().split("|")
                if record[2] == username:  # Check if username matches
                    display_employee_info(*record)
                    return
            print("No records found for you.")
    except FileNotFoundError:
        print("No employee data found.")


# Main function
def main():
    username, role = authenticate_user()
    if not username:
        return

    if role == "admin":
        admin_menu()
    elif role == "employee":
        employee_menu(username)
    else:
        print("Unauthorized access.")


if _name_ == "_main_":
    main()
