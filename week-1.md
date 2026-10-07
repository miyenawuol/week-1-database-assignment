# Week 1 Database Assignment Answer

## Online Music Streaming Platform

This answer provides a complete MySQL database schema, sample data, and a verification query for the **SoundPulse** online music streaming platform.

### 1. Database Script

Save the following code as `answer.sql` and run it in MySQL.

```sql
-- ============================================================================
-- Assignment: Week 1 Database Assignment
-- Topic: SoundPulse - Music Streaming Platform Database
-- File: answer.sql
-- ============================================================================

-- 1. Create the Database
CREATE DATABASE IF NOT EXISTS soundpulse_db;
USE soundpulse_db;

-- ============================================================================
-- 2. Create Tables with Constraints & Relationships
-- ============================================================================

-- Users Table
CREATE TABLE IF NOT EXISTS users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    password_hash VARCHAR(255) NOT NULL,
    subscription_type ENUM('free', 'premium', 'family') DEFAULT 'free',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Artists Table
CREATE TABLE IF NOT EXISTS artists (
    artist_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    bio TEXT,
    country VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Albums Table
CREATE TABLE IF NOT EXISTS albums (
    album_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(150) NOT NULL,
    release_year INT,
    artist_id INT NOT NULL,
    FOREIGN KEY (artist_id) REFERENCES artists(artist_id) ON DELETE CASCADE
);

-- Songs Table
CREATE TABLE IF NOT EXISTS songs (
    song_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(150) NOT NULL,
    duration_seconds INT NOT NULL,
    genre VARCHAR(50),
    album_id INT NOT NULL,
    FOREIGN KEY (album_id) REFERENCES albums(album_id) ON DELETE CASCADE
);

-- Playlists Table
CREATE TABLE IF NOT EXISTS playlists (
    playlist_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(100) NOT NULL,
    user_id INT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE CASCADE
);

-- Playlist_Songs Junction Table (Many-to-Many Relationship)
CREATE TABLE IF NOT EXISTS playlist_songs (
    playlist_id INT NOT NULL,
    song_id INT NOT NULL,
    added_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (playlist_id, song_id),
    FOREIGN KEY (playlist_id) REFERENCES playlists(playlist_id) ON DELETE CASCADE,
    FOREIGN KEY (song_id) REFERENCES songs(song_id) ON DELETE CASCADE
);

-- ============================================================================
-- 3. Insert Sample Data
-- ============================================================================

-- Insert Artists
INSERT INTO artists (name, bio, country) VALUES
('The Midnight', 'Synthwave and electronic band formed in Los Angeles.', 'USA'),
('Daft Punk', 'Legendary electronic music duo.', 'France'),
('Taylor Swift', 'American singer-songwriter across pop, country, and folk.', 'USA');

-- Insert Albums
INSERT INTO albums (title, release_year, artist_id) VALUES
('Endless Summer', 2016, 1),
('Random Access Memories', 2013, 2),
('1989', 2014, 3);

-- Insert Songs
INSERT INTO songs (title, duration_seconds, genre, album_id) VALUES
('Sunset', 326, 'Synthwave', 1),
('Vampires', 317, 'Synthwave', 1),
('Get Lucky', 248, 'Disco/Funk', 2),
('Instant Crush', 337, 'Electronic', 2),
('Style', 231, 'Pop', 3),
('Blank Space', 231, 'Pop', 3);

-- Insert Users
INSERT INTO users (username, email, password_hash, subscription_type) VALUES
('alex_synth', 'alex@example.com', 'hashed_pass_123', 'premium'),
('sarah_j', 'sarah@example.com', 'hashed_pass_456', 'free');

-- Insert Playlists
INSERT INTO playlists (title, user_id) VALUES
('Late Night Drive', 1),
('Pop Party', 2);

-- Add Songs to Playlists
INSERT INTO playlist_songs (playlist_id, song_id) VALUES
(1, 1), -- Sunset in Late Night Drive
(1, 3), -- Get Lucky in Late Night Drive
(2, 5), -- Style in Pop Party
(2, 6); -- Blank Space in Pop Party

-- ============================================================================
-- 4. Sample Verification Query
-- ============================================================================

-- Retrieve all songs with their artist, album, and duration
SELECT 
    s.song_id,
    s.title AS song_title,
    a.title AS album_title,
    ar.name AS artist_name,
    s.genre,
    s.duration_seconds
FROM songs s
JOIN albums a ON s.album_id = a.album_id
JOIN artists ar ON a.artist_id = ar.artist_id;

```

## 2. GitHub Submission Steps

1. **Create the SQL file**

   Save the SQL script above as `answer.sql` in your project folder.

2. **Initialize Git and commit the file**

   Run the following commands in your terminal or command prompt from the project folder:

```bash
git init
git add answer.sql
git commit -m "Add Week 1 MySQL database schema and sample data"
```

3. **Push the project to GitHub**

   Create a public repository named `week-1-database-assignment` on GitHub, then run:

```bash
git remote add origin https://github.com/YOUR_USERNAME/week-1-database-assignment.git
git branch -M main
git push -u origin main
```

> Replace `YOUR_USERNAME` with your GitHub username before running the final command.