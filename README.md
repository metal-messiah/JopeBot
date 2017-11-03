### [Download JopeBot](https://github.com/metal-messiah/JopeBot/releases)

# What *JopeBot* Is, and How It Works
### *JopeBot* is a desktop (Windows, Linux, Mac) twitch chat manager that aims to organize requests based on viewer interaction levels.  It is designed to integrate with [Twitch](http://twitch.com) channels, and use commands from the channel chat room to function.
### It uses the information you must provide in the ./config.json file to connect to your twitch and streamlabs accounts to provide a seamless, self-administering request manager.

### [Configuration](#configuring-the-application-configjson)
### [Running the Application](#running-the-application-1)
### [Using With OBS](#using-with-obs-1)
### [Automatic Backups](#automatic-backups-1)
### [Chat Commands](#chat-commands-1)

# Configuring the Application (config.json)

## Edit ./config.json with a text editing application to configure the application

### Steamlabs Socket API
* Providing your [Streamlabs](https://streamlabs.com) "socket API code" will allow the application to utilize your donations in real time to manage the setlist
* API SETTINGS / API TOKENS / Your Socket API Token

### Twitch Username
* Tells the bot where to connect
* Tells the bot who to give admin privileges to

### Debug Mode
* Must be true or false
* If True, will only show bot messages on your desktop
* If False, will output messages to twitch channel chat room

### Priority Amount
* The value of Donations/Subs/Cheers used to auto-prioritize requests

### Mega Priority Amount
* The value of Donations/Subs/Cheers used to move requests to the very front

### Bot Username
* The Twitch username for the bot to use (Create a new twitch account, do not use your primary channel) 

### Bot Oauth
* The oauth token generated for the bot twitch username, which can be generated [HERE](https://twitchapps.com/tmi/) 

### Mods Use Admin Commands
* Whether mods can use [admin commands](#admin-commands).  If false, only the streamer will have the ability to run admin commands.

### Now Playing Voice
* If set to true, when a request is activated a voice will announce the song

### Tenacious D Now Playing Intros
* If set to true, and "Now Playing Voice" is enabled, will introduce requests with quotes from Tenacious D episodes (vulgar) 

### Skip Null Requests
* Will skip null requests, playing the next request with a valid message

### Days Between Priority Requests
* Allows you to set a grace period between priority requests.  
* Example --> Setting to 0 days allows users unlimited priority requests.
* Example --> Setting to 7 days makes users wait 1 week between priority requests.


# Running the Application
#### After Configuring ./config.json, run JopeBot.exe to activate the application.  A Console will open to provide information about the bot, and where messages will show if DEBUG = true.

# Using With OBS

### The application will automatically generate .txt files in the ./obs directory
* current.txt will show the latest song activated by [!playnext](#play-first-song-in-setlist-removes-request----playnext) or [!playrandom](#play-random-song-in-setlist-removes-request----playrandom)
* played.txt will show the list of songs activated by [!playnext](#play-first-song-in-setlist-removes-request----playnext) or [!playrandom](#play-random-song-in-setlist-removes-request----playrandom)
* poll.txt will only show after the command !poll is activated in chat.  It will outline the choices and will guide the chat.
* priority.txt will show the list of active priority users, assigned through donations, cheers, subs, or otherwise
* setlist.txt will show the number of songs in the setlist, show the currently active request, and preview the next 5 requests.

# Automatic Backups

#### The application will automatically create "saved states" every time a ! command is issued in chat.  These states are stored on the local machine, under ./backup.  They are in JSON format, and can be viewed with any text editor.  They can be used to set JopeBot to that state, using [!restoresetlist](#admin-only-restore-jopebot-from-an-auto-generated-backup-file----restoresetlist-optional-file-name)
#### The application will automatically update a record of the played songs for the session every time a ! command is issued in chat.  This file is located under ./history

# Chat Commands

## General Chat Commands

### Request a Song -- !request MESSAGE
* Adds a request for the user to the setlist
* Only 1 active request allowed per user
* Subsequent requests will overwrite the previous request without losing queue position
* Requests will go into the setlist in the order in which they are received
* Example --> !request Cirice by Ghost


### View your current request status -- !myrequest
* Returns the user's request data
* Description
* isPriority?
* Position in line

### Vote for one of the active poll choices -- !vote 1 or !vote 2
* Vote for the active poll choices
* Only 1 vote per chat user
* see !poll for more information

## Admin Commands

### Assign Priority -- !priority @username
* Bumps the user's request up to the end of the existing priority users' requests
* Priority will get auto-assigned by the bot for subs, re-subs, and 500 bit cheers
* Requests will get bumped up through the setlist in the order in which they are received
* Example --> !priority @jopethemetalmessiah

### Move user's request -- !move USERNAME
* Move user's request to the very front of the setlist
* Example --> !move @jopethemetalmessiah

### Play first song in setlist (removes request) -- !playnext
* Activates the first song request in the queue, and removes it from the setlist.
* Assigns the request to current request, and adds the request to the Played list for tracking
* If "skip null requests" is true, will skip empty requests and instead play the next valid request

### Play random song in setlist (removes request) -- !playrandom
* Activates a RANDOM request in the setlist, and removes it from the setlist.
* Ignores the priority list
* Assigns the request to current request, and adds the request to the Played list for tracking

### Remove the next request in the setlist (does not play) -- !removenext
* Removes the next unplayed request in the queue
* Mainly for use on long standing null requests

### Undo the last played song -- !undo
* Places the last played song back into the setlist

### Start a chat poll between 2 random requests -- !poll
* Grabs 2 random requests for chat users to vote on
* Ignores priority list
* The request which receives the most votes will be pushed to the very front of the
* Poll is active for 30 seconds after the command is written
* see [!vote](#vote-for-one-of-the-active-poll-choices----vote-1-or-vote-2) for more information

### Stop the bot from accepting new requests --  !pause
* Stops new requests from being added to the setlist
* Does not alter existing requests
* Can be undone with [!unpause](#allow-the-bot-to-accept-new-requests-----unpause)

### Allow the bot to accept new requests --  !unpause
* Allows new requests to be added to the setlist

### Restore JopeBot from an auto-generated backup file -- !restoresetlist (optional file name)
* Restores the last saved state generated, which is stored on the host machine under ./backup
* Saved states are generated every time a ! command is issued in chat, keeping the saved state very close to real time
* If a file name is not specified, it will automatically restore TODAY's saved session
* Example 1 - Restore *Today's* Session -- !restoresetlist
* Example 2 - Restore A *Specific* Session -- !restoresetlist backup_10_17_2017

### Setlist Bot Help -- !help
* returns the list of setlist commands