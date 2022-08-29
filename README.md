# SupportChat

SupportChat is a live support ticket system which allows players on your server to quickly get help from a staff member via a private chat.

### Quick Navigation

* [Resource Page (SpigotMC)](https://www.spigotmc.org/resources/60569/)


### Table of Contents

* [General functionality](#general-functionality)
* [The Environment System](#the-environment-system)
* [Installation & Setup](#installation--setup)
* [SupportChat System Properties](#supportchat-system-properties)
* [Configuration](#configuration)
* [Commands & Permissions](#commands--permissions)
* [Bugs & Help](#bugs--help)
* [Epilogue](#epilogue)

## General Functionality

Let's imagine a player *P* playing on your server needs some kind of help. SupportChat makes it possible to establish contact between players and staff members quickly and in an efficient way. *P* could now execute the command `/support Someone burned my house :(`. All logged in staff members will then be informed about the new support request. Staff member *S* opens the GUI for managing support requests via `/requests` and immediately knows what *P* needs help with. *S* accepts *P*'s support request and now they can chat privately. Since *S* is new to the server team, the sever owner *O* wants to check if *S* does a good job on trying to help *P*. *O* also executes `/requests`, clicks on *P*'s request and then on `Listen`. Now *O* can listen to the conversation of *P* and *S* but cannot send any messages in the chat. After they have catched the evil griefer, *S* can end the private chat with `/end` and the support request of *P* was successfully processed.

Unfortunately, *S* was fired for shutting down the entire network... Just kidding - the names of all commands shown in this example **are fully customizable**! (See [Configuration](#command-names) on how to do that)

## The Environment System

SupportChat has several built-in implementations called "environments". Each environment serves a specific use case. Depending on the server version or type, you need to select a suitable environment. Below is an overview of all currently available environments:

| Name of Environment | Developed for | Description                                                                                                                                            | Further Information                                                                               |
|:-------------------:|:-------------:|--------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
|         Ant         |   BungeeCord  | Ant is the default SupportChat environment for BungeeCord. If you want SupportChat to work globally across your entire network, you may choose Ant.    | **Not a standalone!** Only works in combination with `Aide`. (Selected by default for BungeeCord) |
|         Aide        |     Spigot    | Aide is an environment for Spigot which assists the proxy server in game-related tasks.                                                                | **Not a standalone!** Only works in combination with `Ant`. (Selected by default for Spigot)      |
|        Viper        |     Spigot    | Viper is a standalone environment for Spigot. If you only have one independent server or don't want to use SupportChat globally, you may choose Viper. | -                                                                                                 |

## Installation & Setup

1. Download the SupportChat `.jar` file from [the resource page (SpigotMC.org)](https://www.spigotmc.org/resources/60569/)
2. Place the downloaded file in your server's `plugins` directory. 
    * **Important note:** If you wish to use a non-standalone environment, this file must also be placed in each `plugins` directory of each server required for SupportChat to run correctly. For example, if you want to use Ant+Aide, the file must also be installed on all subservers in addition to just the BungeeCord server.
3. Before the servers can be started, SupportChat needs to know which environment to use. This works by setting the system property `supportchat.environment`. This is generally possible in 2 ways (replace 'Ant' with your desired environment):
    1. Set the property from the command line when starting the server  
    (e.g. `java -Dsupportchat.environment="Ant" -jar spigot-1.19.2.jar`).
    3. Create a file called `.properties` in `plugins/SupportChat/` and add the following line to it: `supportchat.environment=Ant`. SupportChat will read this file before initializing the boot procedure.
4. Note that the process described in step 3 needs to be repeated individually for each involved server - **unless** you want to use the default environment for the respective server type (BungeeCord -> Ant; Spigot -> Aide). If the latter applies, step 3 can be skipped completely.
5. Now start all involved servers. SupportChat will then create all files available for configuration in `plugins/SupportChat/`.

## SupportChat System Properties

Here is a list of all system properties for the pre-configuration of SupportChat. As described under [Installation & Setup](#installation--setup), these can be set either via the command line or in the `plugins/SupportChat/.properties` file.

| Property                | Description                                                                                                                                                                  |
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| supportchat.environment | Specifies the environment which SupportChat should use. This can be one from the default set (e.g. "Ant") as well as a path to a class that specifies a custom environment.  |
|  supportchat.plainlogs  | If this property is set, no color codes will be used for console log messages.                                                                                               |
|   supportchat.noprefix  | If this property is set, SupportChat does not automatically append the "[SupportChat]" prefix to log messages.                                                               |
|    supportchat.debug    | If this property is set, debug mode will be enabled.                                                                                                                         |

## Configuration

Available time units:
* `ns` nanoseconds
* `mus` microseconds
* `ms` milliseconds
* `s` seconds
* `m` minutes
* `h` hours
* `d` days

### Config

File: `plugins/SupportChat/config.yml`

| Entry                   | Default Value                              | Description                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|-------------------------|--------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `messages`              | messages/en.yml                            | The path to the file from which SupportChat should read the messages configuration.                                                                                                                                                                                                                                                                                                                                                             |
| `auto-login`            | DISABLED                                   | Determines how players with the permission `supportchat.staff` should be automatically logged in when joining the server:<br>**`DISABLED`**: No automatic login.<br>**`FULL`**: Normal login for each staff member.<br>**`HIDDEN`**: Hidden login for each staff member.<br>**`PERMISSION`**: Login according to highest permission (if the permission `supportchat.staff.hiddenlogin` is present it will be a hidden login, otherwise not). |
| `check-for-updates`     | true                                       | `true` if SupportChat should search for newer versions on startup, `false` if not.                                                                                                                                                                                                                                                                                                                                                          |
| `auto-notification`     | 2m (two minutes)                           | Defines the interval at which all logged in staff members are informed about the remaining unprocessed support requests.<br>Zero + any time unit means disabled.                                                                                                                                                                                                                                                                                |
| `request-delay`         | 10m (ten minutes)                          | Defines the cooldown a player needs to wait at least to create another support request.<br>Zero + any time unit means no cooldown.                                                                                                                                                                                                                                                                                                              |
| `request-auto-deletion` | 1d (one day)                               | Defines the period after which an unprocessed support request will be automatically deleted.<br>Zero + any time unit means infinite.                                                                                                                                                                                                                                                                                                            |
| `reasons.mode`          | RESTRICTED                                 | Determines how the `/supportchatrequest` command should be used:<br>**`DISABLED`**: Specifying a reason is not allowed.<br>**`UNRESTRICTED`**: A reason must be provided but can be freely defined by the player.<br>**`RESTRICTED`**: A reason must be chosen by a predefined set of reasons.                                                                                                                                                 |
| `reasons.reasons`       | - Enter<br>- custom<br>- reasons<br>- here | A predefined set of reasons. Only works with `reasons.mode: RESTRICTED`.                                                                                                                                                                                                                                                                                                                                                                        |

### Command Names

File: `plugins/SupportChat/commands.yml`

| Entry               | Default Value | Description                                                           |
|---------------------|---------------|-----------------------------------------------------------------------|
| `enabled`           | false         | `true` if the custom command names should be applied, `false` if not. |
| `support`           | support       | Custom name for `/supportchatsupport`.                                |
| `supportleave`      | ls            | Custom name for `/supportchatsupportleave`.                           |
| `requests`          | req           | Custom name for `/supportchatrequests`.                               |
| `end`               | endc          | Custom name for `/supportchatend`.                                    |
| `login`             | login         | Custom name for `/supportchatlogin`.                                  |
| `logout`            | logout        | Custom name for `/supportchatlogout`.                                 |
| `leaveconversation` | lc            | Custom name for `/supportchatleaveconversation`.                      |
| `loginlist`         | loginlist     | Custom name for `/supportchatloginlist`.                              |
| `supportchat`       | sc            | Custom name for `/supportchat`.                                       |

### Messages

File: as specified in [config.yml](#config) (default is `plugins/SupportChat/messages/en.yml`

You can either edit the messages directly in the default messages file, or just change the path in the config.yml file. For the latter, SupportChat will create a new message file at the path specified, which can then be edited.

## Commands & Permissions

**Note** that the command names can be changed. See [Configuration](#command-names) for further information.

| Command                       | Arguments         | Permission                                                                           | Description                                                                                                                                                                                                                                                                  |
|-------------------------------|-------------------|--------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /supportchatsupport           | [\<Reason\>]        | supportchat.request                                                                  | Creates a support request. Depending on the configuration, a reason must be specified.                                                                                                                                                                                       |
| /supportchatleave             |                   | supportchat.request                                                                  | Leaves the support queue if a support request has been created before.                                                                                                                                                                                                       |
| /supportchatlogin             | [hidden]          | supportchat.staff supportchat.staff.hiddenlogin (to use `hidden`)                    | Logs the player into the support system. If the argument `hidden` is specified, other logged in players will not see that this player is logged in.                                                                                                                          |
| /supportchatlogout            |                   | supportchat.staff                                                                    | Logs the player out of the support system.                                                                                                                                                                                                                                   |
| /supportchatrequests          |                   | supportchat.staff                                                                    | Opens a GUI to view and manage all current support requests. This command can only be executed by logged in players. It is possible to accept or deny support requests. Ongoing conversations can be listened to using the GUI (with permission `supportchat.staff.listen`). |
| /supportchatend               |                   | supportchat.staff                                                                    | Ends a running conversation. Only the player who accepted the request can do this.                                                                                                                                                                                           |
| /supportchatleaveconversation |                   | supportchat.staff.listen                                                             | Stops listening to the current conversation.                                                                                                                                                                                                                                 |
| /supportchatloginlist         |                   | supportchat.loginlist                                                                | Shows a list of all players who are currently logged in. This will not show the hidden logged in players if the player executing this command does not have the permission `supportchat.staff.hiddenlogin`.                                                                  |
| /supportchat                  | [version\|reload] | supportchat.misc.version (to use `version`) supportchat.misc.admin (to use `reload`) | **version**: Shows the full SupportChat version currently running on the server.<br />**reload**: Reloads the SupportChat configurations (this needs to be done after changing a configuration file).                                                                          |

**Permission** `supportchat.listenernotifications`: If someone joins (or leaves) a conversation to listen to it, everyone with this permission will be notified about it.

## Bugs & Help

If you find any bugs, please report them to me via the [resource page discussion thread](https://www.spigotmc.org/threads/337915/), DM me on Discord or create an Issue right here on GitHub. Thank you!

If something doesn't work correctly or you need any help, don't hesitate and DM me on Discord! I'll try to respond as fast as possible. Suggestions are also welcome.

My Discord: `L3dry#7837`

## Epilogue

SupportChat is a software that I am developing in my spare time and that is completely free of charge. That's a good thing of course because I want to make it accessible to everyone.
Nevertheless, it is a lot of work and therefore I would like to give everyone the opportunity to support me [with a donation](https://www.paypal.com/donate/?hosted_button_id=DG3H89D42KPZC). Thankyou very much! ;)
