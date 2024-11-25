# SQL-x-Pscp
# **Player Information Management API**

This PHP-based API allows for managing player information in a MySQL database. It supports CRUD operations (Create, Read, Update) for fields such as player name, level, credits, banned status, mute status, rank, and more.

## **Table of Contents**
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [API Endpoints](#api-endpoints)
  - [GET - Retrieve Player Information](#get---retrieve-player-information)
  - [POST - Create or Fully Update Player Information](#post---create-or-fully-update-player-information)
  - [PUT - Partially Update Player Information](#put---partially-update-player-information)
- [Database Schema](#database-schema)
- [Error Handling](#error-handling)
- [Testing](#testing)
- [License](#license)

## **Features**

- **GET**: Retrieve player information by `plr-id` or get all players.
- **POST**: Create a new player or fully update existing player information.
- **PUT**: Partially update existing player information.
- Support for additional fields like `muted`, `muted_until`, and `rank`.

## **Requirements**

- PHP 7.0 or higher
- MySQL database
- Apache or any web server that supports PHP
- PHP `mysqli` extension enabled

## **Installation**

### **1. Database Setup**

Create a MySQL database and table for storing player data:

```sql
CREATE DATABASE players;

USE players;

CREATE TABLE player_info (
    plr_id INT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Level INT NOT NULL,
    Credits INT NOT NULL,
    Banned BOOLEAN NOT NULL,
    BanReason VARCHAR(255) DEFAULT NULL,
    banned_until INT DEFAULT NULL,
    muted BOOLEAN NOT NULL DEFAULT FALSE,
    muted_until INT DEFAULT NULL,
    rank VARCHAR(50) NOT NULL DEFAULT 'Member'
);
```

### **2. Configure the PHP Script**

Edit the connection variables in `playerinfo.php` to match your MySQL database configuration:

```php
$servername = "localhost"; // Use the correct hostname
$username = "root";        // Use your MySQL username
$password = "yourpassword"; // Use your MySQL password
$dbname = "players";       // Database name
```

### **3. Deploy the PHP Script**

Place `playerinfo.php` in your web server's root directory:

```bash
/var/www/html/playerinfo.php
```

Ensure your web server is running (e.g., Apache or Nginx).

## **API Endpoints**

### **GET - Retrieve Player Information**

- **URL**: `http://yourserver/playerinfo.php`
- **Method**: GET

**Usage Examples**:

- **Get a single player by ID**:
  ```bash
  curl -X GET "http://yourserver/playerinfo.php?plrid=1"
  ```

- **Get all players**:
  ```bash
  curl -X GET "http://yourserver/playerinfo.php"
  ```

### **POST - Create or Fully Update Player Information**

- **URL**: `http://yourserver/playerinfo.php`
- **Method**: POST
- **Content-Type**: `application/json`

**Data Payload Example**:

```json
{
    "plr-id": 1,
    "Name": "PlayerOne",
    "Level": 20,
    "Credits": 1500,
    "Banned": false,
    "BanReason": null,
    "banned_until": null,
    "muted": false,
    "muted_until": null,
    "rank": "Member"
}
```

**Usage**:

```bash
curl -X POST "http://yourserver/playerinfo.php" -H "Content-Type: application/json" -d '{"plr-id": 1, "Name": "PlayerOne", "Level": 20, "Credits": 1500, "Banned": false, "BanReason": null, "banned_until": null, "muted": false, "muted_until": null, "rank": "Member"}'
```

### **PUT - Partially Update Player Information**

- **URL**: `http://yourserver/playerinfo.php`
- **Method**: PUT
- **Content-Type**: `application/json`

**Data Payload Example**:

```json
{
    "plr-id": 1,
    "Level": 25,
    "Credits": 2000,
    "muted": true,
    "muted_until": 1728000000,
    "rank": "Moderator"
}
```

**Usage**:

```bash
curl -X PUT "http://yourserver/playerinfo.php" -H "Content-Type: application/json" -d '{"plr-id": 1, "Level": 25, "Credits": 2000, "muted": true, "muted_until": 1728000000, "rank": "Moderator"}'
```

## **Database Schema**

The `player_info` table contains the following columns:

| Column       | Type    | Description                      |
|--------------|---------|----------------------------------|
| `plr_id`     | INT     | Unique player ID (Primary Key)   |
| `Name`       | VARCHAR | Player's name                    |
| `Level`      | INT     | Player's current level           |
| `Credits`    | INT     | In-game credits                  |
| `Banned`     | BOOLEAN | If the player is banned          |
| `BanReason`  | VARCHAR | Reason for banning (if any)      |
| `banned_until` | INT   | Unix timestamp when the ban ends |
| `muted`      | BOOLEAN | If the player is muted           |
| `muted_until`| INT     | Unix timestamp when mute ends    |
| `rank`       | VARCHAR | Player's rank (e.g., Member)     |

## **Error Handling**

If there's an error, the API will return a response in the following format:

```json
{
    "status": "error",
    "message": "Description of the error"
}
```

**Common Errors**:

- `Invalid data format for POST request.`
- `Player not found.`
- `Database connection failed.`

## **Testing**

You can use **Postman**, **curl**, or any HTTP client to test the API endpoints.

**Example Test with curl**:

- **GET all players**:
  ```bash
  curl -X GET "http://yourserver/playerinfo.php"
  ```

- **POST a new player**:
  ```bash
  curl -X POST "http://yourserver/playerinfo.php" -H "Content-Type: application/json" -d '{"plr-id": 2, "Name": "PlayerTwo", "Level": 10, "Credits": 500, "Banned": false, "BanReason": null, "banned_until": null, "muted": false, "muted_until": null, "rank": "Member"}'
  ```

- **PUT a partial update**:
  ```bash
  curl -X PUT "http://yourserver/playerinfo.php" -H "Content-Type: application/json" -d '{"plr-id": 2, "Level": 15, "muted": true, "rank": "VIP"}'
  ```

## **License**

This project is open-source and available under the [MIT License](LICENSE).

Feel free to contribute or submit issues!

---

Replace `yourserver` in the examples with the actual server address where the PHP script is hosted. Let me know if you need any further modifications!
