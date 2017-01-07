# Projects
During the course of this class you will work in groups to develop variations
of an application, making use of topics covered in class. Each project will
incorporate features of the previous projects; however, code from previous
projects should be updated when appropriate to utilize newly learned language
features.

## Project 1
Project 1 should make use of topics covered during weeks one though three.

Create a program that allows a user to add, remove, edit, and list to-do items
by presenting the user with a menu similar to the following:

```
Please choose an option:
(1) Add a task.
(2) Remove a task.
(3) Update a task.
(4) List all tasks.
(0) Exit.
```

If the user chooses to add a task, the program should prompt for a description
of the task and add it to the collection of tasks. If the user chooses
to remove a task, the program should ask the user which item to remove and
remove it from the collection of tasks. If the user chooses to update a task,
the program should ask which task will be updated and for a new description of
the task.  The program should loop until the user chooses to exit.  

The program should include methods dedicated to adding, removing, and updating
tasks.

A runnable version of the program is available [here](files/project1.jar). One
way to run this program is by downloading the file and running

```
java -jar Project1.jar
```

from a command prompt or terminal.

Be sure to commit your code and push it to GitHub.


## Project 2
Project 2 should make use of topics covered during weeks four though six.

Create a program that allows a user to add, remove, edit, and list to-do items
similar to the program created for Project 1.  

Individual to-do items should be represented by a class and should allow
a title, description, and priority to be tracked with each to-do item.  
Priority should be specified using a numeric value between 0 and 5 where 5
indicates high priority and 0 indicates low priority.

Users should be prompted to specify the title, description, and priority when
adding tasks.  The user should have the ability to list all tasks or list
only tasks of a specified priority.  

The program should implement exception handling to deal with bad input from
the user and any other exceptions that might arise.  

A runnable version of the program is available [here](files/project2.jar). One
way to run this program is by downloading the file and running

```
java -jar project2.jar
```

from a command prompt or terminal.

Be sure to commit your code and push it to GitHub.  If you would like to
work in teams, use forks and pull requests; individual team members should
submit links to their fork of the repository.

## Project 3
Project 3 should make use of topics covered during weeks seven though eleven.

Modify your code from Project 2 so that the class representing tasks implements
the appropriate interface allowing tasks to be sorted based first on
their priority then on their name.  If two tasks have different priorities,
the task with the greater priority appears before the other task.  If two
tasks have the same priority, then the task whose name would appear first
alphabetically appears before the other task.  If two tasks have the same
priority and their names are the same, then they are "equal" with regard to
the comparison.

Additionally, modify the code from Project 2 to include a custom class
representing a collection of tasks.  This class should implement the
appropriate interface so that a for-each loop can be used to iterate through
all the tasks.  The order in which the tasks are returned for the for-each loop
is up to you.

Be sure to commit your code and push it to GitHub.  If you would like to
work in teams, use forks and pull requests; individual team members should
submit links to their fork of the repository.

## Project 4
Project 4 should make use of topics covered during weeks twelve though fifteen.

Modify your code from project 3 to support adding, updating, removing, listing 
tasks, and listing task of a certain priority to/from a remote server using 
JSON and a RESTful API.  The URL used to access the server will be provided 
later in class.  

Creating and updating a task will require sending JSON to the server in the
following form:

```json
{
  "title":"Car",
  "priority": 2,
  "body":"Wash the car"
}
```

Retrieving an individual task will require that your program be able to create
objects from the following JSON string:

```json
{
  "body": "mow grass",
  "done": false,
  "uuid": "05494bb0a-e8c8-49b8-a584-ccdcc4f0e1f8",
  "priority": 1,
  "title": "grass"
}
```

Retrieving all tasks will require that your program be able to process the
following JSON string:

```json
{
  "todos": [
    {
      "body": "Walk the dog",
      "done": false,
      "uuid": "5494bb0a-e8c8-49b8-a584-ccdcc4f0e1f8",
      "priority": 3,
      "title": "dog"
    },
    {
      "body": "Prepare dinner",
      "done": false,
      "uuid": "f012b899-3819-44fd-91c7-2f06fddffb99",
      "priority": 1,
      "title": "Dinner"
    },
    {
      "body": "Take out the trash",
      "done": false,
      "uuid": "ae9e6e7a-758f-43c4-8b0f-1c17c8ba05fd",
      "priority": 5,
      "title": "Trash"
    }
  ]
}
```
Due to issues with Gradle and IntelliJ, the program does not need to support 
user interaction using the console; instead, you can hard-code examples of 
adding a task, updating a task, removing a task, listing tasks, and listing 
tasks of a given priority.