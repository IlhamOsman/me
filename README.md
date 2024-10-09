print("I'm thinking of a number from 1 to {limit}")
    
    attempts = 0
    while True:
        guess = int(input("Your guess: "))  
        attempts += 1
        
        if guess < gussingnumber:
            print("Too low.")
        elif guess > gussingnumber:
            print("Too high.")
        else:
            print("You guessed it in {attempts} tries.")
            break

if name == "main":
    main()
