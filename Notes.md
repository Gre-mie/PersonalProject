# Bot Project Notes:

boot.dev personal project

Open page preview in VScode: **ctrl + shift + v**

# Terms

**guild**, Discord.Guild object -  a discord community server.

# Structure (database)

```
Database (global)
    |
    |_________ Guilds (wrapper) objects
                        |
                        |___________ Sugestion Objects
                        |
                        |___________ Question Objects
```

# Object Structures

## Database Object

An object with methods to add a community when the bot is invited to a server.

Acts as a database object for other objects

- [on_guild_join Event](https://discordpy.readthedocs.io/en/stable/api.html?highlight=on_guild_join#discord.on_guild_join): this event is triggured when the bot creates or joins (is invited to) a server
- [Discord.Guild](https://discordpy.readthedocs.io/en/stable/api.html?highlight=on_guild_join#discord.Guild): this object stores informations such as the owner object, owner ID, the guild ID, channels/text channels, 

**Note**: There is a property called `system_channel` that might be helpful for sending logs. If no system channel is set it returns `None`.

### Uage

```
if not database:
    database = Database()
```

### Variables:

```
Database(void):
{
    __guilds {
        guild (discord) name, str: Guild Object,        # might need to use: 
                                                        # guild (discord) ID, int: {guild (discord) name, str: Guild Object}
                                                        # due to needing to check the ID before adding the object, 
                                                        # might make it quicker, not sure
    }
}
```

### Methods:

`add_guild(Discord.Guild)`<br>
Triggered by Event: **on_guild_join**<br>
Add a community to the communities dict, saves the Discord.Guild object in the Guild (wrapper) Object
    
- should check if the new guild is already in the dict

```
# not Discord.Guild ID in self.guilds.keys() 
    # add Guild Object(Discord.Guild, )           # display form, used to populate guild object variables
```

---
    
`remove_guild(Discord.Guild)`<br>
Might be triggered when the bot is kicked from the server

```
# if Discord.Guild Id in self.guilds.keys():
    # delete all objects inside the guild object arrays (may be recursive)
    # delete the guild object ?
    # delete the guild from the array ?
```

---

`update_guilds()`<br>
Iterates through the guilds array calling Guild Objet methods to update them

- Delete any object with delete:true
    + Guild._delete(questions array)
    + Guild._delete(suggestions array)
    + set date of last automatic update on guild object

---

`debug_print_guilds_tree()`<br>
prints the guilds in a tree like structure with minimal info

```
#        guilds
#            |________ guild name: guild ID
#            |
#            |________ guild name: guild ID
#
#        guilds: 2
```

---

## Guild (wrapper) Object

An object to store a Discord community and their questions.<br>
(Hoping this will help to solve the problem of not having a database and prevent differnt commities adding/deleting/editing/reciving the same list of questions)

**Note**: The Discord.Guild object cannot be extended. This means the object must be saved in a custom object (wrapper) to add new variables and methods

### Usage

This is a wrapper used to extend a Discord.Guild object.<br>
Guid Objects are created using `database.add_guild(Discord.Guild)` to add a server to the database

### Variables:

```
Guild(Discord.Guild, # variables set by the contents of a from):
{
    __guild = Discord.Guild object (wrapped)

    # These are NOT needed since the self.guild object will already have them. !! They are here as a reminder only !!
    # guild (discord) Name= string
    # guild (discord) ID= int
    # discord guild owners ID= int


    __command_channel= channel ID           # possible needs a form
    __question_channel= channel ID          # possible needs a form
    __suggestion_channel= channel ID        # posisble needs a form
    __admin_role= role ID                   # will need to be set by owner eg. set_amin_role(admin role name) # possible needs a form

    __sugestions= {}
    __quesitons= {}
}
```

### Methods

SCOPE CREEP:<br>
`set_channel(channel, option=None)`<br>
could be called using the contents of a from<br>
triggured with !c command by owner/admin<br>
sets a channel variable
    
- returns error if a valid channel is not given "!c [channel name] or !c --[option] [channel name]"

```
    # if --question option is given:
        # sets the question_channel to given channel ID
    # elif --sugestion option is given:
        # sets the sugestion_channel to given channel ID
    # elif --admin-role option is given:
        # sets the admin_role to given role ID
    # else no valid option is given:
        # sets the commands_channel to given channel ID
```

---

SCOPE CREEP:<br>
`_get_questions_info()`<br>
returns a dictinary of information about the questions array

```
dict{
    length: int length of array,
    questions to be asked: int number of questions that are deleted:False && history:False,
    quesitons asked: int number of quesitons that are history:ture,
    questions set to be deleted: int number of questions that are deleted:True 
    date of last cleanup: date the arrays were last automatically updated
}
```

---

SCOPE CREEP:<br>
`_calculate_time_from_pressent(old_time, time_now)`<br>
Helper function<br>
calculates the amount of time between old_time and time_now<br>
returns string

```
# example of what can be returned

"45 mins"
"2 days, 15 mins"
"1 week, 4 days, 6 mins"
"4 weeks +"
```

---

SCOPE CREEP:<br>
`_get_suggestion_info()`<br>
returns a dictionary of information about the suggestions array

```
Returns dict{
    length: int length of the array,
    oldest submission: {id, date submited, # SCOPE CREEP: amount of time between submission and now}, 
    latest submission: {id, date submitted, # SCOPE CREEP: amount of time between submission and now},
    submissions set to be deleted: submissions set to delete:true
    date of last cleanup: date the arrays were last automatically updated
}
```

---

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


**Possible structure**:

```
questions = {ID:Object, ID:Object, ID:Object}
suggested questions = {ID:Object, ID:Object, ID:Object}
```

Questions should be saved in an array to allow them to be easily edited and deleted.

!c [channel name]: sets a channel to be used for commands by the owner/admin. Untill this is set, all channels can use commands and possibly seen by everyone
!c --question [channel name]: sets which channel will be used for asking questions. Until this is set, only the owner/admin will be able to use commands
!c --sugestion [channel name]: sets which channel will be used for submitting suggested quesitons by Discord members. If this is not set, the question channel will be used instead
!c --admin-role [role name]: sets the ID for the admin role using the admin role name

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
