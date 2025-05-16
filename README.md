# Tribal-Wars-API

A comprehensive resource for accessing and utilizing the Tribal Wars (Die Stämme/Plemiona) game API. This repository provides documentation and tools for developers looking to build applications that interact with Tribal Wars data.

**Keywords**: Tribal Wars, Die Stämme, Plemiona, InnoGames, game API, village data, player statistics, tribe information, conquest history, TWStats, Tribalwars API, plemiona.pl API

## Repository Description

This repository serves as a central resource for developers and players interested in working with Tribal Wars game data. It includes:

- 📚 **Complete API Documentation**: Detailed explanation of all available endpoints and data formats
- 🔄 **Data Structure Guides**: Comprehensive information about the structure and relationships between game entities
- 🌐 **Multi-Server Support**: Instructions for accessing data across different language versions (DE, US, PL, etc.)
- 🔍 **Conquest Tracking**: Documentation for monitoring village ownership changes and tribe territory

Whether you're building statistical tools, map viewers, conquest trackers, or alliance management systems, this repository provides the foundational knowledge needed to access and process game data efficiently.

## Table of Contents
- [Overview](#overview)
- [Data Access Guidelines](#data-access-guidelines)
- [Server URLs](#server-urls)
- [Configuration Endpoints](#configuration-endpoints)
- [World Data Files](#world-data-files)
  - [Village Data](#village-data)
  - [Player Data](#player-data)
  - [Tribe Data](#tribe-data)
  - [Conquest Data](#conquest-data)
  - [Extended Conquest Data](#extended-conquest-data)
  - [Opponent Statistics](#opponent-statistics)
- [Recent Conquests Endpoint](#recent-conquests-endpoint)
- [World Map Background](#world-map-background)
- [Data Relationships](#data-relationships)

## Overview

Tribal Wars provides public access to game data for each world through standardized text files and API endpoints. This data can be used to create external tools, statistical analyses, map viewers, and other third-party applications. The data includes information about villages, players, tribes, conquests, and various ranking statistics.

## Data Access Guidelines

To ensure these resources remain available for everyone, please follow these guidelines:

- **Update Frequency**: All data files are updated approximately once per hour
- **Download Limit**: Fetch data no more than once per hour per file
- **Use Compression**: Always use the `.gz` compressed versions when available
- **Random Timing**: For automated scripts, use random timing within the hour
- **Respect Rate Limits**: Excessive requests may result in IP address blocking
- **Data Format**: All text data is comma-separated with URL-encoded strings

## Server URLs

Server URLs follow a standardized pattern based on world ID and language domain:

```
http://[world-id].[language-domain]/[endpoint]
```

**Examples:**
- German: `http://de68.die-staemme.de/`
- US: `http://us1.tribalwars.us/`
- Polish: `http://pl158.plemiona.pl/`

### Listing Available Servers

To obtain a complete list of active servers for a specific language domain:

```
http://www.die-staemme.de/backend/get_servers.php
```

This endpoint returns a serialized PHP array where each key is a server identifier (e.g., "de199" or "dec1") and each value is the corresponding server URL.

## Configuration Endpoints

The game provides several XML-formatted configuration files that contain important settings and parameters for each world:

### General Configuration

**Endpoint:** `/interface.php?func=get_config`

This XML file contains various world settings including:
- World speed
- Unit speed
- Morale system configuration
- Building settings
- Game mechanics parameters
- Ranking calculation formulas

### Building Information

**Endpoint:** `/interface.php?func=get_building_info`

This XML file contains details about all buildings available in the world:
- Maximum and minimum levels
- Resource costs (wood, stone, iron)
- Population requirements
- Build time
- Growth factors for each value

### Unit Information

**Endpoint:** `/interface.php?func=get_unit_info`

This XML file contains details about all units available in the world:
- Build time
- Population requirements
- Speed
- Attack and defense values
- Carrying capacity

## World Data Files

All world data files are available in both plain text and gzip-compressed format:
- `/map/filename.txt`
- `/map/filename.txt.gz` (recommended for efficiency)

Each file contains data in a specific format with comma-separated values. All text strings are URL-encoded using the PHP `urlencode()` function.

### Village Data

**File path:** `/map/village.txt.gz`

This file contains information about all villages in the game world, including coordinates, ownership, and points.

**Data structure:**
```
village_id, name, x_coordinate, y_coordinate, player_id, points, rank
```

**Field descriptions:**
- `village_id`: Unique identifier for the village
- `name`: Village name (URL-encoded)
- `x_coordinate`: X-coordinate on the map (horizontal position)
- `y_coordinate`: Y-coordinate on the map (vertical position)
- `player_id`: ID of the player who owns the village (0 for barbarian villages)
- `points`: Village points value
- `rank`: Village rank or bonus ID (varies by server version)

**Example entry:**
```
1,036+Doniu,609,485,6135387,7811,0
```

### Player Data

**File path:** `/map/player.txt.gz`

This file contains information about all players in the game world, including tribe membership and statistics.

**Data structure:**
```
player_id, name, tribe_id, village_count, points, rank
```

**Field descriptions:**
- `player_id`: Unique identifier for the player
- `name`: Player name (URL-encoded)
- `tribe_id`: ID of the tribe the player belongs to (0 if not in a tribe)
- `village_count`: Number of villages owned by the player
- `points`: Player's total points
- `rank`: Player's position in the world rankings

**Example entry:**
```
1759,Yasic,683,2,9287,1915
```

### Tribe Data

**File path:** `/map/ally.txt.gz`

This file contains information about all tribes (alliances) in the game world.

**Data structure:**
```
tribe_id, name, tag, member_count, village_count, points, all_points, rank
```

**Field descriptions:**
- `tribe_id`: Unique identifier for the tribe
- `name`: Full tribe name (URL-encoded)
- `tag`: Tribe tag/abbreviation (URL-encoded)
- `member_count`: Number of players in the tribe
- `village_count`: Total number of villages owned by tribe members
- `points`: Tribe points (calculated for ranking)
- `all_points`: Total combined points of all tribe members
- `rank`: Tribe's position in the world rankings

**Example entry:**
```
2,Walhalla,WHL,24,17,41850,41850,94
```

### Conquest Data

**File path:** `/map/conquer.txt.gz`

This file contains a historical record of all village conquests since the world began.

**Data structure:**
```
village_id, timestamp, new_owner_id, old_owner_id
```

**Field descriptions:**
- `village_id`: ID of the conquered village
- `timestamp`: Time of conquest (Unix timestamp)
- `new_owner_id`: Player ID of the new owner
- `old_owner_id`: Player ID of the previous owner (0 for barbarian villages)

**Example entry:**
```
10867,1741018220,698850251,0
```

### Extended Conquest Data

**File path:** `/map/conquer_extended.txt.gz`

An extended version of the conquest data that includes tribe information.

**Data structure:**
```
village_id, timestamp, new_owner_id, old_owner_id, old_tribe_id, new_tribe_id, village_points
```

**Field descriptions:**
- `village_id`: ID of the conquered village
- `timestamp`: Time of conquest (Unix timestamp)
- `new_owner_id`: Player ID of the new owner
- `old_owner_id`: Player ID of the previous owner (0 for barbarian villages)
- `old_tribe_id`: Tribe ID of the previous owner (0 if none)
- `new_tribe_id`: Tribe ID of the new owner (0 if none)
- `village_points`: Points value of the village at the time of conquest

**Example entry:**
```
10867,1741018220,698850251,0,0,2,52
```

### Opponent Statistics

Several files track "Opponents Defeated" statistics for both players and tribes:

**Player statistics files:**
- `/map/kill_all.txt.gz`: Total opponent points defeated
- `/map/kill_att.txt.gz`: Opponent points defeated as attacker
- `/map/kill_def.txt.gz`: Opponent points defeated as defender
- `/map/kill_sup.txt.gz`: Opponent points defeated as supporter

**Tribe statistics files:**
- `/map/kill_all_tribe.txt.gz`: Total opponent points defeated by tribe
- `/map/kill_att_tribe.txt.gz`: Opponent points defeated by tribe as attackers
- `/map/kill_def_tribe.txt.gz`: Opponent points defeated by tribe as defenders

**Data structure (all kill files):**
```
rank, id, points_defeated
```

**Field descriptions:**
- `rank`: Position in the specific leaderboard
- `id`: Player ID or tribe ID (depending on the file)
- `points_defeated`: Number of opponent points defeated

**Example entry from kill_def.txt:**
```
1,849230226,18343544
```

## Additional Information

### Recent Conquests

### Recent Conquests Endpoint

To get only conquests since a specific time:
```
/interface.php?func=get_conquer&since=UNIX_TIMESTAMP
```
or the extended version:
```
/interface.php?func=get_conquer_extended&since=UNIX_TIMESTAMP
```

**Important Note**: The timestamp must be within the last 24 hours. If you use a timestamp older than 24 hours, you will receive an error:
```
ERR ONLY_ONE_DAY_AGO - Es sind nur die Adelungen der letzten 24 Stunden abrufbar
```
(Translation: "Only conquests from the last 24 hours are available")

Example usage:
```
https://pl213.plemiona.pl/interface.php?func=get_conquer&since=1747434771
```

The endpoint returns data in the same format as the respective conquer.txt files.

## World Map Background

**File path:** `/world.dat.gz`

This file contains information about the terrain type of each map coordinate, which is used to render the world map background.

The file has the following characteristics:
- Contains 1000 rows with 1000 bytes each (approximately 1MB in size)
- Each byte represents a map coordinate's terrain type
- Coordinates can be accessed using: `offset = y_coordinate * 1000 + x_coordinate`

**Terrain types and values:**
- `0-3`: Grass variations ("gras1.png", "gras2.png", "gras3.png", "gras4.png")
- `8-11`: Mountain variations ("berg1.png", "berg2.png", "berg3.png", "berg4.png")
- `12`: Lake ("see.png")
- `16-31`: Forest variations ("forest0000.png" through "forest1111.png")

The terrain graphics can typically be found at:
```
https://dsde.innogamescdn.com/asset/45436e33/graphic/map_new/[image_name]
```

## Data Relationships

The data across different files is interconnected through unique identifiers, forming a hierarchical relationship:

```
Village → Player → Tribe
```

These relationships work as follows:

1. **Village to Player**: 
   - Each village in `village.txt` has a `player_id` field
   - This ID can be used to look up the owner in `player.txt`

2. **Player to Tribe**:
   - Each player in `player.txt` has a `tribe_id` field
   - This ID can be used to look up the tribe in `ally.txt`

3. **Conquest Tracking**:
   - `conquer.txt` tracks village ownership changes
   - Links villages (`village_id`) to old and new owners (`old_owner_id` and `new_owner_id`)
   - Extended version also links to tribes (`old_tribe_id` and `new_tribe_id`)

This relationship structure allows you to:
- Find all villages belonging to a specific player
- Find all players belonging to a specific tribe
- Find all villages belonging to a specific tribe (indirectly)
- Track conquest history for any village, player, or tribe

To perform complex operations with this data, you'll need to join information from multiple files, either using database operations or application logic.

---

**Note**: This documentation is based on the public API information available as of May 2025. API details may change over time - always check the official game forums for the most up-to-date information.
