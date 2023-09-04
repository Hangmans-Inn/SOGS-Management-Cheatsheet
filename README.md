# SOGS Management Cheatsheet

**Version**: 1.0.1 | **Last Updated**: August 26, 2023

# SOGS Management Cheatsheet

> **Introduction**: A comprehensive guide for managing Session rooms using the SOGS (Session Open Group Server) tool. This cheatsheet is designed for administrators and covers commands, permissions, user management, and practical usage examples. **Prerequisites**: Basic understanding of command-line operations.

## Table of Contents

- [[#Introduction|Introduction]]
- [[#Commands Overview|Commands Overview]]
- [[#Room Management|Room Management]]
- [[#User & Permissions Management|User & Permissions Management]]
- [[#Advanced Options|Advanced Options]]
- [[#SQL Database Management|SQL Database Management]]
- [[#Examples|Examples]]

---

## Commands Overview

> **Tip**: Initiate the SOGS help prompt with `sudo sogs --help` to display a suite of available commands and options.

### Basic Information

- `--version, -V`: View the program's current version.
- `--help, -h`: Pull up the help menu.

### Room Operations

- `--add-room TOKEN`: Instantiate a room using the specified token.
- `--delete-room TOKEN`: Erase the room associated with the provided token.
- `--list-rooms, -L`: Display all existing rooms and essential statistics.

### Moderation and Permissions

- `--add-moderators`: Assign Session ID(s) as room moderators.
- `--delete-moderators`: Revoke moderator status from the specified Session ID(s).
- `--users`: Define specific users for permissions configuration.
- `--add-perms`: Grant defined permissions to selected users or rooms.
- `--remove-perms`: Rescind specific permissions from chosen users or rooms.
- `--clear-perms`: Reset permissions to default settings.

---

## Room Management

### Room Creation and Customization

To create a new room:

```shell
sogs --add-room TOKEN --name 'Desired Room Name' --description 'Brief Room Description'
```
To eliminate a room:

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

### Managing Permissions

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

### Global and Room-specific Operations

The `--rooms` option permits applying commands globally (`*`), to a specific room, or across all rooms (`+`).

### Visibility Control for Moderators/Admins

The visibility of moderators/admins can be toggled using `--visible` (public) or `--hidden` (private).

### Database Operations

- `--initialize`: Initiates the database and private key. 
- `--upgrade, -U`: Executes necessary database upgrades.

---

## SQL Database Management

### Room Information Table

Here is a table summarizing the room IDs, room names, and room tokens:

```markdown
# | Room ID | Room Name            | Room Token     |
# |---------|----------------------|----------------|
# | RoomID1 | The Artisan's Atelier| artisanatelier |
# | RoomID2 | Hangman's Tavern     | hangmantavern  |
# | RoomID3 | Hentai Wing - {NSFW} | hentaiwing     |
# | RoomID4 | Horny Jail - {NSFW}  | hornyjailed    |
# | RoomID5 | Meme Palace          | memepalace     |
# | RoomID6 | Mystic's Cove        | mysticcove     |
# | RoomID7 | Tech Tavern          | techtavern     |
# | RoomID8 | Testing Room         | testingroom    |
# | RoomID9 | Warden's Office      | office         |
```

### SQL Commands for Room Management

- **List All Room Tokens and Room IDs**:
  ```sql
  SELECT id, token FROM rooms;
  ```

- **Finding User ID from Session ID**:
  ```sql
  SELECT id FROM users WHERE session_id = "YourSessionIDHere";
  ```

- **Temporarily Ban a User for 30 Seconds**:
  ```sql
  INSERT INTO user_ban_futures (room, user, at, banned) VALUES (RoomID, UserID, strftime('%s','now') + 30, 1);
  ```

- **Unban a User**:
  ```sql
  DELETE FROM user_ban_futures WHERE room = RoomID AND user = UserID;
  ```

- **Temporarily Ban a User for 1 Week**:
  ```sql
  INSERT INTO user_ban_futures (room, user, at, banned) VALUES (RoomID, UserID, strftime('%s','now') + 604800, 1);
  ```

- **List All Users in a Specific Room**:
  ```sql
  SELECT u.session_id
  FROM users u
  JOIN room_users ru ON u.id = ru.user
  WHERE ru.room = RoomID;
  ```

### Troubleshooting

- **Database Lock Issue**: If you encounter a "database is locked" error, it's likely that another process is accessing the database. Make sure to close other SQLite sessions or applications that might be using the database.

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

> **Note**: Always ensure you're using valid session IDs and room tokens when executing commands.

## External Resources

- [Official SOGS Documentation](#)
- [SOGS Community Forum](#)

## Contact Information

For further questions, please contact [Your Name](mailto:your.email@example.com).

#Tags: #SOGS #Management #Cheatsheet #Commands #Permissions #FAQs #Troubleshooting
