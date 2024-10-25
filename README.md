# Movie List Program

movies = ["Monty Python and the Holy Grail", "On the Waterfront", "Cat on a Hot Tin Roof"]

def show_menu():
    print("\nCOMMAND MENU")
    print("list - List all movies")
    print("add - Add a movie")
    print("del - Delete a movie")
    print("exit - Exit program")

def list_movies():
    if len(movies) == 0:
        print("No movies to display.")
    else:
        for i, movie in enumerate(movies, start=1):
            print(f"{i}. {movie}")

def add_movie():
    name = input("Name: ")
    movies.append(name)
    print(f"{name} was added.")

def delete_movie():
    try:
        number = int(input("Number: "))
        if 1 <= number <= len(movies):
            movie = movies.pop(number - 1)
            print(f"{movie} was deleted.")
        else:
            print("Invalid movie number.")
    except ValueError:
        print("Invalid input. Please enter a valid movie number.")

def main():
    show_menu()
    while True:
        command = input("\nCommand: ").lower()
        if command == "list":
            list_movies()
        elif command == "add":
            add_movie()
        elif command == "del":
            delete_movie()
        elif command == "exit":
            print("Bye!")
            break
        else:
            print("Not a valid command. Please try again.")

if _name_ == "_main_":
    main()
