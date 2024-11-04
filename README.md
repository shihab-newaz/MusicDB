# Music Streaming Platform Database

A comprehensive SQL database implementation for a music streaming platform, featuring user management, playlists, artists, albums, songs, podcasts, and more.

## Table of Contents
- [Overview](#overview)
- [Database Schema](#database-schema)
- [Features](#features)
- [Implementation Details](#implementation-details)
- [SQL Operations](#sql-operations)
- [PL/SQL Components](#plsql-components)
- [Setup and Installation](#setup-and-installation)
- [Usage Examples](#usage-examples)

## Overview

This project implements a relational database system for a music streaming platform with features similar to Spotify or Apple Music. It includes functionality for:
- User management
- Playlist creation and management
- Music library organization
- Artist and album tracking
- Podcast support
- Genre classification
- Play count tracking

## Database Schema

### Tables Structure
1. **USERS_TABLE**
   - Primary key: user_id
   - Fields: username, email, password, country, age

2. **PLAYLIST**
   - Primary key: playlist_id
   - Fields: name, creation_date, original_owner

3. **PLAYS**
   - Foreign keys: user_id, playlist_id
   - Fields: last_accessed

4. **ARTIST**
   - Primary key: artist_id
   - Fields: name, country

5. **ALBUM**
   - Primary key: album_id
   - Foreign key: artist_id
   - Fields: name, release_year

6. **SONG**
   - Primary key: song_id
   - Foreign key: album_id
   - Fields: title, duration, release_date, ratings

7. **FEATURES**
   - Foreign keys: playlist_id, song_id
   - Fields: song_played

8. **PODCASTS**
   - Foreign key: artist_id
   - Fields: podcast_id, duration, type

9. **GENRE**
   - Foreign key: song_id
   - Fields: name, description

## Features

### Core Functionality
- User account management
- Playlist creation and modification
- Music library organization
- Artist and album tracking
- Song play count tracking
- Genre classification
- Podcast management

### Database Operations
- CRUD operations for all entities
- Complex queries and joins
- Aggregation functions
- Transaction management
- Data integrity constraints
- Triggers for data validation

## Implementation Details

### Table Relationships
- One-to-Many relationship between Artists and Albums
- One-to-Many relationship between Albums and Songs
- Many-to-Many relationship between Playlists and Songs
- One-to-Many relationship between Artists and Podcasts

### Data Integrity
- Foreign key constraints with CASCADE delete
- NOT NULL constraints on essential fields
- Unique constraints on identifiers
- Date validation for release dates
- Rating range validation

## SQL Operations

### Basic Operations
```sql
-- Select with conditions
SELECT title, duration, ratings FROM SONG;
SELECT name, artist_id, release_year FROM ALBUM WHERE album_id=31;

-- Range search
SELECT * FROM features WHERE song_played BETWEEN 1000000 AND 10000000;

-- Pattern matching
SELECT podcast_id, duration, artist_id FROM PODCASTS
WHERE type LIKE '%Monologue%';
```

### Advanced Queries
```sql
-- Aggregate functions
SELECT MAX(song_played), MIN(song_played) FROM FEATURES;
SELECT COUNT(age), SUM(age), AVG(age) FROM USERS_TABLE;

-- Group by with Having
SELECT artist_id, COUNT(album_id) from ALBUM
GROUP BY artist_id HAVING COUNT(album_id)>1;

-- Nested queries
SELECT name, country FROM ARTIST WHERE artist_id IN 
(SELECT artist_id FROM ARTIST WHERE artist_id>1 AND artist_id<6);
```

## PL/SQL Components

### Procedures
```sql
-- Example procedure for getting song popularity
CREATE or REPLACE PROCEDURE get_song_popularity IS 
    times_played FEATURES.song_played%type;
    song_title  VARCHAR(100);
    popularity VARCHAR(100);
    -- Procedure implementation
END;
```

### Functions
```sql
-- Example function for average rating
CREATE OR REPLACE FUNCTION avg_rating RETURN NUMBER IS
    avg_ratings SONG.ratings%type;
    -- Function implementation
END;
```

### Triggers
```sql
-- Example trigger for release year validation
CREATE OR REPLACE TRIGGER trig BEFORE INSERT OR UPDATE ON ALBUM
FOR EACH ROW
DECLARE
    min_release_year constant number(4):=2000;
    -- Trigger implementation
END;
```

## Setup and Installation

1. Run the table creation scripts in order:
```sql
-- Drop existing tables
DROP TABLE GENRE;
DROP TABLE PODCASTS;
-- ... (other drop statements)

-- Create tables
CREATE TABLE USERS_TABLE (
    -- table definition
);
-- ... (other create statements)
```

2. Run the sample data insertion scripts:
```sql
INSERT INTO USERS_TABLE(user_id, username, email, password, country, age) 
VALUES (1,'SHIHAB', 'shihab.newaz.563@gmail.com', '1234','Bangladesh',23);
-- ... (other insert statements)
```

## Usage Examples

### Adding a New User
```sql
INSERT INTO USERS_TABLE(user_id, username, email, password, country, age) 
VALUES (5, 'NewUser', 'user@email.com', 'pass123', 'USA', 25);
```

### Creating a New Playlist
```sql
INSERT INTO PLAYLIST(playlist_id, name, creation_date, original_owner) 
VALUES(14, 'My Playlist', SYSDATE, 'Username');
```

### Adding Songs to Playlist
```sql
INSERT INTO FEATURES(playlist_id, song_id, song_played)
VALUES(14, 201, 0);
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
