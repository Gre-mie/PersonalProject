# Bot Project Notes:

boot.dev personal project
Open page preview in VScode: **ctrl + shift + v**

# Structure (database)

```
Global Dict 
    |
    |_________ community objects Dict
                        |
                        |___________ Sugestions Objects
                        |
                        |___________ Questions Objects
```

# Object Structures

## Global Dict

An object with methods to add a community when the bot is invited to a server and populate the feilds

### Variables:

```
Global Dict(void):
{
    communities {
        community (discord) name: Community Object,
    }
}
```

### Methods:

add community(community data)

- add a community to the communities dict

## Community Object

An object to store a Discord community and their questions.
(Hoping this will help to solve the problem of not having a database and prevent differnt commities adding/deleting/editing/reciving the same list of questions)

### Variables:

```
Community():
{
    community (discord) Name= string
    community (discord) ID= number
    discord community owners discord info= {
        name: discord username= string
        ID: 
    }
}
```

### Methods



# COMMANDS

Responses to commands should be sent to the channel the command was used in unless its a special command for debugging

## Owner and Admin Commands

Only the discord owner or admins should be able to use these commands:

!cmd : list all available commands
!cmd --help : list all available commands with an explanation of what the command does

**(FOR NOW!)**
Questions should be objects with data such as

### Suggested questions Object:

suggestion ID=
author=
question=

### Question Object:

question ID=
original author=suggestion ID.author
original question=suggestion ID.question
content=original question
who added the question (owner/mod)
date/time question was first added
date/time questions was last edited
who last edited the question (owner/mod)
date/time question was asked=NULL (question has not been asked yet)
history=False (should be False if date/time question was asked == NULL)
if the question has been deleted=False
deleted by=NULL
revived by=NULL 

### Question Object Methods:

edit: changes the question content and set 
who last edited the question=
date/time question was last edited=



(**!!!Problem!!! LATER: POSSIBLE SCOPE CREEP**: It would be best if questions were put into a database to prevent multiple discord communities that invited the bot from adding questions to it that are used for all communities.
I will probably need help with setting up a way for discord community owners to create their own database for their questions)


Possible structure:
questions = {ID:Object, ID:Object, ID:Object}
suggested questions = {ID:Object, ID:Object, ID:Object}



Questions should be saved in an array to allow them to be easily edited and deleted.

!q : print all questions in the question array

!q --info : prints all questions not deleted with the most relevant meta data for each question object
!q --info --all : prints all questions not deleted with all the meta data for each question object
!q --info <question ID> : prints all the meta data for a specified question object

!q --history : print all questions with asked=True
!q --deleted : prints all questions that are set to deleted

!q --revive : brings back a deleted question, deleted=False

!q --add <suggestions ID>: add a question from the suggested questions dict to the questions dict, should bring up a form, auto completed using the suggested questions 

!q --edit <question ID> : edit a question, should bring up a form with a text field with the question in its current state auto completed ready to be edited

!q --delete <question ID> : should print the question with some meta data and ask for confirmation for this question to be deleted. question should not be permanently deleted, but instead have deleted=True
!q --delete -f <question ID> : should print the question with all meta data and ask for confirmation for this question to be deleted. **THERE SHOULD BE A CLEAR WARNING THAT THIS WILL DELETE THE QUESTION PERMINENTLY**

When printing questions the question Id should be displayed to allow for questions to be picked 

## Public Commands

ANYONE can use these commands:

!question : suggest a question, should bring up a form for users to submit suggested questions, question should be added to the suggested questions dict
(SCOPE CREEP: permissions for links and images to be added might need to be set)

!commands : prints a list of commands available to users
!commands --help : prints a list of commands available to users with an explanation of what each command does


----------------------------------------

When any form is submitted it should give some kind of confirmation message of success/failure.
For users this message should possibly thank them for their submission or explain a problem with their submission (SCOPE CREEP: On failure it would be nice if a users submission was printed as a message so they could at least copy and past what they had and edit it in a new submission)
