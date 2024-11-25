class Login:
    """
    Class to represent a user login with three properties: user_id, password, and authorization.
    """
    def _init_(self, user_id, password, authorization):
        self.user_id = user_id
        self.password = password
        self.authorization = authorization


def read_user_data(filename):
    """
    Reads user data from the file and stores it in a list of Login objects.
    """
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
    """
    Collects and validates user data, then writes it to the file.
    """
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

        # Append new user to the list and write to file
        new_user = Login(user_id, password, authorization_code)
        user_list.append(new_user)

        with open(filename, "a") as file:
            file.write(f"{user_id}|{password}|{authorization_code}\n")

        print("User data successfully added!\n")


def login_process(user_list):
    """
    Handles the login process:
    - Verifies user ID and password.
    - Creates a Login object if credentials are valid.
    - Grants access based on authorization code.
    """
    user_id = input("Enter User ID: ").strip()
    password = input("Enter Password: ").strip()

    # Validate user ID and password
    for user in user_list:
        if user.user_id == user_id:
            if user.password == password:
                print("\nLogin Successful!")
                print(f"User ID: {user.user_id}, Password: {user.password}, Authorization: {user.authorization}")
                
                # Check authorization
                if user.authorization == "Admin":
                    print("\nAuthorization: Admin - Full Access Granted.")
                    display_all_user_data(user_list)  # Admin can display all data
                elif user.authorization == "User":
                    print("\nAuthorization: User - Limited Access Granted.")
                    display_user_data(user)  # User can only view their own data
                return
            else:
                print("\nError: Incorrect Password.")
                return

    print("\nError: User ID not found.")
    exit()


def display_user_data(user):
    """
    Displays the details of a single user.
    """
    print("\n--- User Details ---")
    print(f"User ID: {user.user_id}")
    print(f"Password: {user.password}")
    print(f"Authorization Code: {user.authorization}")


def display_all_user_data(user_list):
    """
    Displays details of all users along with totals.
    """
    print("\n--- All User Data ---")
    total_users = len(user_list)
    for user in user_list:
        print(f"User ID: {user.user_id}, Password: {user.password}, Authorization: {user.authorization}")

    print(f"\nTotal Users: {total_users}")


def main():
    filename = "user_data.txt"

    # Step 1: Read user data from the file and store in a list of Login objects
    user_list = read_user_data(filename)

    # Step 2: Collect and store new user data
    validate_and_store_user_data(filename, user_list)

    # Step 3: Start the login process
    login_process(user_list)


if _name_ == "_main_":
    main()
