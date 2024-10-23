# Step 1: Efficient function definitions
def get_input(prompt):
    return input(prompt)

def get_numeric_input(prompt):
    return float(input(prompt))

def calculate_pay(hours, rate, tax_rate):
    gross = hours * rate
    tax = gross * tax_rate
    net = gross - tax
    return gross, tax, net

def display_employee_info(name, hours, rate, gross, tax_rate, tax, net):
    print(f"\nEmployee: {name}\nHours Worked: {hours}\nHourly Rate: {rate}")
    print(f"Gross Pay: {gross}\nTax Rate: {tax_rate * 100}%\nTax: {tax}\nNet Pay: {net}\n")

def display_totals(count, total_hours, total_gross, total_tax, total_net):
    print(f"\nEmployees Processed: {count}\nTotal Hours: {total_hours}")
    print(f"Total Gross Pay: {total_gross}\nTotal Tax: {total_tax}\nTotal Net Pay: {total_net}\n")

# Step 2: Efficient main loop
def main():
    employee_count, total_hours, total_gross, total_tax, total_net = 0, 0, 0, 0, 0

    while True:
        if get_input("Enter 'End' to stop or press Enter to continue: ").lower() == "end":
            break
        
        name = get_input("Enter employee name: ")
        hours = get_numeric_input("Enter hours worked: ")
        rate = get_numeric_input("Enter hourly rate: ")
        tax_rate = get_numeric_input("Enter tax rate (in %): ") / 100
        
        gross, tax, net = calculate_pay(hours, rate, tax_rate)
        
        display_employee_info(name, hours, rate, gross, tax_rate, tax, net)
        
        employee_count += 1
        total_hours += hours
        total_gross += gross
        total_tax += tax
        total_net += net

    display_totals(employee_count, total_hours, total_gross, total_tax, total_net)

# Step 3: Execute main function
if _name_ == "_main_":
    main()
