# CapstoneProjectFiles
This Python program, will read data from two text files called user.txt and tasks.txt will perform different operations to allow a small business manage tasks assigned to each member of the team.
## Installation
You will need PyCharm to run this project.\
Copy the files task_manager.py, tasks.txt and user.txt to the root file of your new project in PyCharm
## Usage
For the development of this software, datetime import, loops, validations and a menu were created. \
The components will be described below highlighting their purposes.
### Login section
#### Read user file
```Python
# declare variables for empty strings, open the user document and reads from it, every line
# in the document will be read with a for loop replacing any extra commas and empty space.
# the lines will be split and will be converted to a dictionary to link
# the relevant username with password
user_credentials = ""
display_statistics = ""
with open('./user.txt', 'r') as f:
    for line in f:
        user_credentials = user_credentials + line.replace(",", "")
        user_credentials_list = user_credentials.split()
    user_credentials_dic = {user_credentials_list[i]: user_credentials_list[i + 1] for i in
                            range(0, len(user_credentials_list), 2)}
```
#### Validate user loggin in
```Python
# while loop that will run as long as a condition is met and brakes out, two variables are declared to
# store the username and password to be logged in. conditional statement to check if the username and password
# entered matches any key and value on the dictionary. If yes another conditional statement is created to check
# if the user logging in is admin or not. If not conditional statement will check either if the
# username or password is incorrect and display the appropriate message. The loop will stop if the username
# and password pass validation
while True:

    # user input is converted to lower case and no blank spaces.
    username = input("Enter your username: ").lower().strip()
    password = input("Enter your password: ").lower().strip()

    # if statement to validated username and password in dictionary against user input
    if (username, password) in user_credentials_dic.items():

        # if validated and username is no 'admin', validated variables to true and a new variable that with hold
        # the user in session
        if username != "admin":
            print("\nLogged in...!\n")
            validated_username = True
            validated_password = True
            user_session = username

        else:

            # if username is 'admin', validated variables to true and a new variable that with hold
            # the user in session, also a new variable will hold the new option to be displayed in menu
            print("\nLogged in...!\n")
            validated_username = True
            validated_password = True
            user_session = username
            display_statistics = "vs - View Statistics"

        break

    elif username not in user_credentials_dic and password in user_credentials_dic.values():

        # display message if wrong username entered
        print("\nWrong username\n"
              "Please try again!!!\n")

    elif username in user_credentials_dic and user_credentials_dic[username] != password:

        # display message if wrong password entered
        print("\nWrong password\n"
              "Please try again!!!\n")

    else:

        # display message if both username and password are entered
        print("\nWrong username and password\n"
              "Please try again!!!\n")
```
### Main program after user loggin
#### Menu
```Python
# while loop that will run as long as a username and password has been validated. condition is met and brakes out.
# if validated a menu will be displayed to the user, if user is 'admin' a new option will be added to the existing menu
while validated_username and validated_password:

    # presenting the menu to the user and
    # making sure that the user input is converted to lower case and no blank spaces.
    # if user logged in is 'admin', menu will present a new option 'vs - View Statistics'
    menu = input(f'''Select one of the following Options below:
r - Registering a user
a - Adding a task
va - View all tasks
vm - view my task
{display_statistics}
e - Exit
: ''').lower().strip()

    # conditional statement to check if user input is 'r' or 'a' or 'va' or 'vm' or 'e' and will
    # execute the appropriate command
    if menu == 'r':

        # while loop that will run as long as a condition is met and brakes out
        while True:

            # conditional statement to check if user is admin.
            if user_session == "admin":

                # if user is admin, will be allowed to register new user. User input is converted to lower case
                # and no blank spaces.
                new_username = input("Enter the new username: ").lower().strip()
                new_password = input("Assign a password: ").lower().strip()
                confirm_password = input("Confirm password: ").lower().strip()

                # conditional statement to check if admin user has confirmed password entered, and print
                # the appropriate message
                if new_password != confirm_password:

                    print("\nPasswords do not match.\n"
                          "Please try again from the beginning!!!\n")

                # conditional statement to check that admin user is not registering the same user twice,
                # and print the appropriate message
                elif new_username in user_credentials_dic:

                    print("\nSorry...\n"
                          "User already registered!!!\n"
                          "Please try again from the beginning!!!\n")

                else:

                    # if password is confirmed, print a message and write the data entered on user.txt file
                    # on a new line
                    print("\nNew user Registered!!!\n")
                    with open('user.txt', 'a') as f:
                        f.write(str("\n" + new_username) + ", " + confirm_password)

                    break

            else:

                # if user is not 'admin' print an appropriate message
                print("\nSorry, you need to log in as 'admin' to register new users!!!\n")

                break

    elif menu == 'a':
        # User input is converted to lower case
        person_assigned_to_task = input("\nEnter the name of the person whom the task is assigned to: ").lower()

        # conditional statement to check if user input is a key in dictionary
        if person_assigned_to_task in user_credentials_dic:

            # user input is store in a variable and converted to a title
            task_title = input("Enter the title of the task: ").title()

            # user input is store in a variable
            task_description = input("Describe the task been assigned: ")

            # user input will be converted to the correct date format, using date library
            task_due_date = input("Task due date dd/mm/yyyy: ")
            task_due_date = datetime.datetime.strptime(task_due_date, "%d/%m/%Y").strftime("%d %B %Y")

            # current system date will be obtained and converted to a user-friendly format using
            # datetime library
            current_date = date.today()
            task_current_date = current_date.strftime("%d %B %Y")

            # user input is store in a variable
            task_completion = "No"

            # print a message and write the data entered on tasks.txt file
            # on a new line
            print("\nNew task created!!!\n")
            with open('tasks.txt', 'a') as f:
                f.write(str("\n" + person_assigned_to_task) + ", " + task_title + ", " + task_description + ", " +
                        task_current_date + ", " + task_due_date + ", " + task_completion)

        else:
            # if user input not in dictionary, print an appropriate message and shows all available usernames
            print("\nSorry...\n"
                  "This user is not register in our system!!!\n"
                  "Tasks can only be assigned to the below users\n"
                  f"{', '.join(user_credentials_dic.keys())}\n")

    elif menu == 'va':

        #  opens and read tasks.txt file. For loop to iterate each line in the file and split it where a comma ',' is
        #  print an appropriate user-friendly message with the contents of tasks.txt file
        with open('./tasks.txt', 'r') as f:
            for line in f:
                tasks_list = line.split(", ")

                print(f"""{'_' * 100}\n
Task:              {tasks_list[1]}
Assigned to:       {tasks_list[0]}
Date assigned:     {tasks_list[3]}
Due date:          {tasks_list[4]}
Task completed?:   {tasks_list[5]}
Task description:   
{tasks_list[2]}             
{'_' * 100}\n
""")

    elif menu == 'vm':

        # opens and read tasks.txt file. For loop to iterate each line in the file and split it where a comma ',' is
        # print all tasks assigned to the user in session in an appropriate user-friendly format
        with open('./tasks.txt', 'r') as f:
            no_task = 0
            for line in f:
                tasks_list = line.split(", ")

                # conditional statement that will print the tasks only for the user in session if any
                if user_session == tasks_list[0]:
                    no_task = 1
                    print(f"""{'_' * 100}\n
Task:              {tasks_list[1]}
Assigned to:       {tasks_list[0]}
Date assigned:     {tasks_list[3]}
Due date:          {tasks_list[4]}
Task completed?:   {tasks_list[5]}
Task description:   
{tasks_list[2]}             
{'_' * 100}\n
""")

            else:
                # if no tasks assigned an appropriate message will be displayed
                if no_task == 0:

                    print("\nWe could not find any tasks assigned to you... yet!!!\n")

    elif menu == "vs":

        # creation of two variables with value '0'
        task_count = 0
        users_count = 0

        # opens and read tasks.txt file, and count all lines in the file
        with open('./tasks.txt', 'r') as f:
            for line in f:
                task_count += 1

        # opens and read user.txt file, and count all lines in the file
        with open('./user.txt', 'r') as f:
            for line in f:
                users_count += 1

        # print statistics of all the files to the admin user in an appropriate user-friendly format
        print("\nAdmin panel Statistics")
        print(f"""{'_' * 50}\n        
Total number of users:        {users_count}
Total numbers of tasks:       {task_count}             
{'_' * 50}\n
""")

    elif menu == 'e':

        # print appropriate message and exit the program
        print('Goodbye!!!')
        exit()

    else:

        # print appropriate message if user made a wrong choice from the menu
        print("You have made a wrong choice, Please Try again\n")
```
## Contributing
Pull requests are welcome. For any changes, please open an issue first to discuss what you would like to change.
Please make sure to update tests as appropriate.
## Credits
© Angel De Sousa
