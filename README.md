# Part 1: Create a text file with movie titles
def create_movies_file():
    movies = ["Cat on a Hot Tin Roof", "On the Waterfront", "Monty Python and the Holy Grail"]
    with open("movies.txt", "w") as file:
        for movie in movies:
            file.write(movie + "\n")

# Part 2: Display the command menu
def display_menu():
    print("COMMAND MENU")
    print("list - List all movies")
    print("add  - Add a movie")
    print("del  - Delete a movie")
    print("exit - Exit program")

# Part 3: Load movies from file into a list
def load_movies():
    try:
        with open("movies.txt", "r") as file:
            return [line.strip() for line in file.readlines()]
    except FileNotFoundError:
        return []

# Part 4: Functionality for listing, adding, and deleting movies
def list_movies(movies):
    if not movies:
        print("No movies found.")
    else:
        for i, movie in enumerate(movies, start=1):
            print(f"{i}. {movie}")

def add_movie(movies):
    new_movie = input("Movie: ")
    if new_movie:
        movies.append(new_movie)
        save_movies(movies)
        print(f"{new_movie} was added.")

def delete_movie(movies):
    try:
        number = int(input("Number: "))
        if 1 <= number <= len(movies):
            removed_movie = movies.pop(number - 1)
            save_movies(movies)
            print(f"{removed_movie} was deleted.")
        else:
            print("Invalid movie number.")
    except ValueError:
        print("Invalid input. Please enter a number.")

def save_movies(movies):
    with open("movies.txt", "w") as file:
        for movie in movies:
            file.write(movie + "\n")

# Main program logic
def main():
    create_movies_file()  # Part 1: Create initial file with movies
    movies = load_movies()  # Part 3: Load movies from the file

    while True:
        display_menu()
        command = input("Command: ").strip().lower()
        
        if command == "list":
            list_movies(movies)
        elif command == "add":
            add_movie(movies)
        elif command == "del":
            delete_movie(movies)
        elif command == "exit":
            print("Bye!")
            break
        else:
            print("Not a valid command. Please try again.")

if __name__== "_main__":
    main()
