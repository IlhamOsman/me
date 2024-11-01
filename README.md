# Import datetime for date handling
from datetime import datetime

# Function to input and return dates
def get_dates():
    """Prompts user for 'from' and 'to' dates and returns them."""
    return input("Enter from date (mm/dd/yyyy): "), input("Enter to date (mm/dd/yyyy): ")

# Function to calculate pay details
def calculate_pay(hours, rate, tax_rate):
    """Calculates gross pay, tax, and net pay based on hours, rate, and tax rate."""
    gross = hours * rate
    tax = gross * tax_rate
    return gross, tax, gross - tax  # Returning multiple values efficiently

# Function to display individual employee information
def display_employee_info(from_date, to_date, name, hours, rate, gross, tax_rate, tax, net):
    """Displays details for a single employee."""
    print(f"\nFrom Date: {from_date} To Date: {to_date}")
    print(f"Employee Name: {name}")
    print(f"Hours Worked: {hours}, Hourly Rate: ${rate:.2f}")
    print(f"Gross Pay: ${gross:.2f}, Income Tax Rate: {tax_rate * 100:.1f}%")
    print(f"Income Tax: ${tax:.2f}, Net Pay: ${net:.2f}\n")

# Function to display total summary
def display_totals(totals):
    """Displays cumulative totals for all employees processed."""
    print("\n--- Totals ---")
    print(f"Total Number of Employees: {totals['employee_count']}")
    print(f"Total Hours Worked: {totals['total_hours']}")
    print(f"Total Gross Pay: ${totals['total_gross']:.2f}")
    print(f"Total Income Tax: ${totals['total_tax']:.2f}")
    print(f"Total Net Pay: ${totals['total_net']:.2f}\n")

# Main function
def main():
    employees = []  # List to store each employee's data
    totals = {  # Dictionary to store cumulative totals
        'employee_count': 0,
        'total_hours': 0,
        'total_gross': 0,
        'total_tax': 0,
        'total_net': 0
    }

    while True:
        if input("Enter 'End' to stop or press Enter to continue: ").strip().lower() == "end":
            break

        # Gather employee data
        from_date, to_date = get_dates()
        name = input("Enter employee name: ")
        hours = float(input("Enter hours worked: "))
        rate = float(input("Enter hourly rate: "))
        tax_rate = float(input("Enter tax rate (in %): ")) / 100

        # Calculate pay details
        gross, tax, net = calculate_pay(hours, rate, tax_rate)

        # Display individual employee information
        display_employee_info(from_date, to_date, name, hours, rate, gross, tax_rate, tax, net)

        # Append employee data to list
        employees.append({
            'from_date': from_date,
            'to_date': to_date,
            'name': name,
            'hours': hours,
            'rate': rate,
            'gross': gross,
            'tax_rate': tax_rate,
            'tax': tax,
            'net': net
        })

        # Update totals in the dictionary
        totals['employee_count'] += 1
        totals['total_hours'] += hours
        totals['total_gross'] += gross
        totals['total_tax'] += tax
        totals['total_net'] += net

    # Display totals at the end of the loop
    display_totals(totals)

# Run the program
if __name__ == "__main__":
    main()
