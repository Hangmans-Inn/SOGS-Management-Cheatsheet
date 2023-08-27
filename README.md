 # SOGS Management Cheatsheet

 A comprehensive guide for managing Session rooms using the SOGS (Session Open Group Server) tool.

 ## Table of Contents

 - [[#Introduction|Introduction]]
 - [[#Commands Overview|Commands Overview]]
- [[#Room Management|Room Management]]
 - [[#User & Permissions Management|User & Permissions Management]]
 - [[#Advanced Options|Advanced Options]]
 - [[#Examples|Examples]]

 ---

 ## Introduction

 SOGS, short for Session Open Group Server, is a powerful tool designed to aid administrators in managing Session rooms. This cheatsheet furnishes details about commands, permissions, user management, and practical usage examples to ease your SOGS administration endeavors.

 ---

 ## Commands Overview

 Initiate the SOGS help prompt with `sudo sogs --help`. This will display a suite of available commands and options.

 Here's a breakdown of the principal commands and their applications:

 - **Basic Information**:
   - `--version, -V`: View the program's current version.
   - `--help, -h`: Pull up the help menu.

 - **Room Operations**:
   - `--add-room TOKEN`: Instantiate a room using the specified token.
   - `--delete-room TOKEN`: Erase the room associated with the provided token.
   - `--list-rooms, -L`: Display all existing rooms and essential statistics.
 
 - **Moderation and Permissions**:
   - `--add-moderators`: Assign Session ID(s) as room moderators.
   - `--delete-moderators`: Revoke moderator status from the specified Session ID(s).
   - `--users`: Define specific users for permissions configuration.
   - `--add-perms`: Grant defined permissions to selected users or rooms.
   - `--remove-perms`: Rescind specific permissions from chosen users or rooms.
   - `--clear-perms`: Reset permissions to default settings.

 ---

 ## Room Management

 ### Creation and Customization

 To create a new room:

 ```shell
 sogs --add-room TOKEN --name 'Desired Room Name' --description 'Brief Room Description'
 ```

 Eliminate a room with:

 ```shell
sogs --delete-room TOKEN
 ```

 ---

 ## User & Permissions Management

 ### Assigning Moderators and Administrators

 Moderators can be allocated to specific rooms, or as global moderators applicable to all rooms:

 - **To specific rooms**: 
   ```shell
   sogs --rooms ROOM_TOKEN_1 ROOM_TOKEN_2 --add-moderators SESSION_ID_1 SESSION_ID_2
   ```

 - **As global moderators**:
   ```shell
   sogs --add-moderators SESSION_ID --rooms + --visible
   ```

 ### Permissions

 The SOGS tool defines four permissions: Read (`r`), Write (`w`), Upload (`u`), and Access (`a`). These permissions can be granted, revoked, or reset.

 - **Granting Permissions**:
   ```shell
   sogs --add-perms rw --rooms ROOM_TOKEN
   ```

 - **Revoking Permissions**:
   ```shell
   sogs --remove-perms u --rooms ROOM_TOKEN
   ```

 - **Resetting Permissions**:
   ```shell
   sogs --clear-perms rwua --rooms ROOM_TOKEN
   ```

 ---

 ## Advanced Options

 - **Global and Room-specific Operations**: The `--rooms` option permits applying commands globally (`*`), to a specific room, or across all rooms (`+`).

 - **Visibility Control for Moderators/Admins**: The visibility of moderators/admins can be toggled using `--visible` (public) or `--hidden` (private).

 - **Database Operations**: 
   - `--initialize`: Initiates the database and private key. 
   - `--upgrade, -U`: Executes necessary database upgrades.

 ---

 ## Examples

 1. **Adding a Room**:
    ```shell
    sogs --add-room myRoomToken --name 'My Room' --description 'This is a sample room.'
    ```

 2. **Adding Admins to Specific Rooms**:
    ```shell
    sogs --rooms roomA roomB --admin --add-moderators SESSION_ID_1 SESSION_ID_2
    ```

 3. **Assigning Global Moderators**:
    ```shell
    sogs --add-moderators SESSION_ID --rooms + --visible
    ```

 4. **Setting Room Permissions**:
    ```shell
    sogs --add-perms rw --remove-perms u --rooms '*'
    ```

 ---

 **Note**: Always ensure you're using valid session IDs and room tokens when executing commands.

 #Tags: #SOGS #Management #Cheatsheet #Commands #Permissions
