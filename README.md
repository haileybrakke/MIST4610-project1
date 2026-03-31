# MIST4610-project1

# Group

Name: Group 9

Members:

Hailey Brakke (https://github.com/haileybrakke/MIST4610-project1) 

Will Federer (put github link)

Summer Sayedzada (put github link)

Tony Jimenez (https://github.com/tonyj010/MIST4610)

Ja’Khiyan Dowdy (https://github.com/jakhiyan0219-hub/Project-1.git)

# Scenario Description

Our client is the Operations Manager for the NBA Eastern Conference. The Eastern Conference league office supports reporting and coordination across all 15 Eastern Conference teams. The office needs to track team participation, player rosters, game schedules and results, coaching assignments, and player performance throughout the season. Currently, this information is stored across multiple spreadsheets and public sources, making it difficult to monitor roster changes over time, analyze team and player performance, and generate reliable operational reports. This project creates a centralized relational database to manage publicly available basketball operations data and improve reporting efficiency. 

# Data Model

Model Explanation: The model represents the NBA league structure by linking conferences, divisions, and teams together. Each team belongs to a division, and each division belongs to a conference. The model also tracks player participation through the Roster table, which connects players to teams and seasons, allowing players to move between teams over time while maintaining historical records.

Game data includes the date, location (arena), season, type (regular season or postseason), and competing teams (home and away). Each game is linked to one arena, one season, and one game type, while teams can participate in many games. 

Player performance is recorded in the PlayerGameStats table, which connects players to games and stores basic statistics such as points. Coaching history is tracked using the TeamCoachAssignment table, which links coaches to teams over time.

This database supports queries looking to answer questions involving team structure, player movement, game outcomes, and individual performance across seasons. However, it is not capable of answering questions related to detailed play-by-play data, advanced statistics, financial information, or teams beyond the Eastern Conference.

<img width="973" height="695" alt="Screenshot 2026-03-30 at 8 03 58 PM" src="https://github.com/user-attachments/assets/95b9ce65-d330-47c9-9a41-b57d9f405c93" />

# Data Dictionary
<img width="688" height="312" alt="Screenshot 2026-03-31 at 10 05 44 AM" src="https://github.com/user-attachments/assets/20c4005c-06a2-43de-a6a2-0857462071ed" />
<img width="665" height="461" alt="Screenshot 2026-03-31 at 10 06 34 AM" src="https://github.com/user-attachments/assets/b8fc697e-ed83-4f9b-84b9-7838e69db537" />
<img width="673" height="521" alt="Screenshot 2026-03-31 at 10 07 15 AM" src="https://github.com/user-attachments/assets/19f663c5-8468-460f-8151-50340317e04e" />





# Query Matrix 

# Queries

# Database Information:

Name: al_hb11648

# Database Code

CREATE TABLE Conference (
    conference_id INT PRIMARY KEY,
    conference_name VARCHAR(45) NOT NULL
);

CREATE TABLE Division (
    division_id INT PRIMARY KEY,
    division_name VARCHAR(45) NOT NULL,
    conference_id INT NOT NULL,
    FOREIGN KEY (conference_id) REFERENCES Conference(conference_id)
);

CREATE TABLE Team (
    team_id INT PRIMARY KEY,
    team_name VARCHAR(45) NOT NULL,
    city VARCHAR(45) NOT NULL,
    abbreviation VARCHAR(10) NOT NULL,
    division_id INT NOT NULL,
    FOREIGN KEY (division_id) REFERENCES Division(division_id)
);

CREATE TABLE Season (
    season_id INT PRIMARY KEY,
    season_label VARCHAR(20) NOT NULL,
    start_date DATETIME NOT NULL,
    end_date DATETIME NOT NULL
);

CREATE TABLE Arena (
    arena_id INT PRIMARY KEY,
    arena_name VARCHAR(60) NOT NULL,
    city VARCHAR(45) NOT NULL,
    state VARCHAR(45) NOT NULL
);

CREATE TABLE GameType (
    game_type_id INT PRIMARY KEY,
    game_type_name VARCHAR(45) NOT NULL
);

CREATE TABLE Game (
    game_id INT PRIMARY KEY,
    game_date DATETIME NOT NULL,
    home_score INT NOT NULL,
    away_score INT NOT NULL,
    arena_id INT NOT NULL,
    season_id INT NOT NULL,
    game_type_id INT NOT NULL,
    home_team_id INT NOT NULL,
    away_team_id INT NOT NULL,
    FOREIGN KEY (arena_id) REFERENCES Arena(arena_id),
    FOREIGN KEY (season_id) REFERENCES Season(season_id),
    FOREIGN KEY (game_type_id) REFERENCES GameType(game_type_id),
    FOREIGN KEY (home_team_id) REFERENCES Team(team_id),
    FOREIGN KEY (away_team_id) REFERENCES Team(team_id)
);

CREATE TABLE Player (
    player_id INT PRIMARY KEY,
    first_name VARCHAR(45) NOT NULL,
    last_name VARCHAR(45) NOT NULL,
    primary_position VARCHAR(20) NOT NULL,
    height INT NOT NULL,
    weight INT NOT NULL,
    birthdate DATE NOT NULL
);

CREATE TABLE PlayerStatus (
    status_id INT PRIMARY KEY,
    player_status_name VARCHAR(45) NOT NULL
);

CREATE TABLE Roster (
    roster_id INT PRIMARY KEY,
    jersey_number INT NOT NULL,
    start_date DATETIME NOT NULL,
    end_date DATETIME NULL,
    player_id INT NOT NULL,
    team_id INT NOT NULL,
    season_id INT NOT NULL,
    status_id INT NOT NULL,
    FOREIGN KEY (player_id) REFERENCES Player(player_id),
    FOREIGN KEY (team_id) REFERENCES Team(team_id),
    FOREIGN KEY (season_id) REFERENCES Season(season_id),
    FOREIGN KEY (status_id) REFERENCES PlayerStatus(status_id)
);

CREATE TABLE Coach (
    coach_id INT PRIMARY KEY,
    first_name VARCHAR(45) NOT NULL,
    last_name VARCHAR(45) NOT NULL
);

CREATE TABLE TeamCoachAssignment (
    team_coach_id INT PRIMARY KEY,
    role VARCHAR(45) NOT NULL,
    start_date DATETIME NOT NULL,
    end_date DATETIME NULL,
    team_id INT NOT NULL,
    coach_id INT NOT NULL,
    FOREIGN KEY (team_id) REFERENCES Team(team_id),
    FOREIGN KEY (coach_id) REFERENCES Coach(coach_id)
);

CREATE TABLE PlayerGameStats (
    player_id INT NOT NULL,
    game_id INT NOT NULL,
    team_id INT NOT NULL,
    points INT NOT NULL,
    rebounds INT NOT NULL,
    assists INT NOT NULL,
    minutes_played INT NOT NULL,
    steals INT NOT NULL,
    blocks INT NOT NULL,
    turnovers INT NOT NULL,
    PRIMARY KEY (player_id, game_id),
    FOREIGN KEY (player_id) REFERENCES Player(player_id),
    FOREIGN KEY (game_id) REFERENCES Game(game_id),
    FOREIGN KEY (team_id) REFERENCES Team(team_id)
);

#Adding Data

INSERT INTO Conference (conference_id, conference_name)
VALUES
(1, 'Eastern Conference');

INSERT INTO Division (division_id, division_name, conference_id)
VALUES
(1, 'Atlantic', 1),
(2, 'Central', 1),
(3, 'Southeast', 1);

INSERT INTO Team (team_id, team_name, city, abbreviation, division_id)
VALUES
(1, 'Boston Celtics', 'Boston', 'BOS', 1),
(2, 'New York Knicks', 'New York', 'NYK', 1),
(3, 'Philadelphia 76ers', 'Philadelphia', 'PHI', 1),
(4, 'Brooklyn Nets', 'Brooklyn', 'BKN', 1),
(5, 'Milwaukee Bucks', 'Milwaukee', 'MIL', 2),
(6, 'Cleveland Cavaliers', 'Cleveland', 'CLE', 2),
(7, 'Indiana Pacers', 'Indianapolis', 'IND', 2),
(8, 'Miami Heat', 'Miami', 'MIA', 3),
(9, 'Orlando Magic', 'Orlando', 'ORL', 3),
(10, 'Atlanta Hawks', 'Atlanta', 'ATL', 3);

INSERT INTO Season (season_id, season_label, start_date, end_date)
VALUES
(1, '2022-2023', '2022-10-01 00:00:00', '2023-06-30 00:00:00'),
(2, '2023-2024', '2023-10-01 00:00:00', '2024-06-30 00:00:00'),
(3, '2024-2025', '2024-10-01 00:00:00', '2025-06-30 00:00:00');

INSERT INTO Arena (arena_id, arena_name, city, state)
VALUES
(1, 'TD Garden', 'Boston', 'MA'),
(2, 'Madison Square Garden', 'New York', 'NY'),
(3, 'Wells Fargo Center', 'Philadelphia', 'PA'),
(4, 'Barclays Center', 'Brooklyn', 'NY'),
(5, 'Fiserv Forum', 'Milwaukee', 'WI'),
(6, 'Rocket Mortgage FieldHouse', 'Cleveland', 'OH'),
(7, 'Gainbridge Fieldhouse', 'Indianapolis', 'IN'),
(8, 'Kaseya Center', 'Miami', 'FL'),
(9, 'Kia Center', 'Orlando', 'FL'),
(10, 'State Farm Arena', 'Atlanta', 'GA');

INSERT INTO GameType (game_type_id, game_type_name)
VALUES
(1, 'Regular Season'),
(2, 'Postseason');

INSERT INTO PlayerStatus (status_id, player_status_name)
VALUES
(1, 'Active'),
(2, 'Inactive'),
(3, 'Injured');

INSERT INTO Coach (coach_id, first_name, last_name)
VALUES
(1, 'Joe', 'Mazzulla'),
(2, 'Tom', 'Thibodeau'),
(3, 'Nick', 'Nurse'),
(4, 'Jordi', 'Fernandez'),
(5, 'Doc', 'Rivers'),
(6, 'Kenny', 'Atkinson'),
(7, 'Rick', 'Carlisle'),
(8, 'Erik', 'Spoelstra'),
(9, 'Jamahl', 'Mosley'),
(10, 'Quin', 'Snyder');

INSERT INTO TeamCoachAssignment (team_coach_id, role, start_date, end_date, team_id, coach_id)
VALUES
(1, 'Abe Adams', '2022-10-01 00:00:00', NULL, 1, 1),
(2, 'Ben Brown', '2022-10-01 00:00:00', NULL, 2, 2),
(3, 'Carter Crown', '2022-10-01 00:00:00', NULL, 3, 3),
(4, 'Dan Davidson', '2022-10-01 00:00:00', NULL, 4, 4),
(5, 'Emily Emerson', '2022-10-01 00:00:00', NULL, 5, 5),
(6, 'Fred Frans', '2022-10-01 00:00:00', NULL, 6, 6),
(7, 'Grace Gregory', '2022-10-01 00:00:00', NULL, 7, 7),
(8, 'Hannah Ham', '2022-10-01 00:00:00', NULL, 8, 8),
(9, 'Ian Island', '2022-10-01 00:00:00', NULL, 9, 9),
(10, 'Joe Jones', '2022-10-01 00:00:00', NULL, 10, 10);

INSERT INTO Player (player_id, first_name, last_name, primary_position, height, weight, birthdate)
VALUES
(1, 'Jayson', 'Tatum', 'SF', 80, 210, '1998-03-03'),
(2, 'Jaylen', 'Brown', 'SG', 78, 223, '1996-10-24'),
(3, 'Derrick', 'White', 'PG', 76, 190, '1994-07-02'),

(4, 'Jalen', 'Brunson', 'PG', 74, 190, '1996-08-31'),
(5, 'Josh', 'Hart', 'SG', 76, 215, '1995-03-06'),
(6, 'Mikal', 'Bridges', 'SF', 78, 209, '1996-08-30'),

(7, 'Joel', 'Embiid', 'C', 84, 280, '1994-03-16'),
(8, 'Tyrese', 'Maxey', 'PG', 74, 200, '2000-11-04'),
(9, 'Paul', 'George', 'SF', 80, 220, '1990-05-02'),

(10, 'Cam', 'Thomas', 'SG', 76, 210, '2001-10-13'),
(11, 'Nic', 'Claxton', 'C', 83, 215, '1999-04-17'),
(12, 'Cameron', 'Johnson', 'SF', 80, 210, '1996-03-03'),

(13, 'Giannis', 'Antetokounmpo', 'PF', 83, 243, '1994-12-06'),
(14, 'Damian', 'Lillard', 'PG', 74, 195, '1990-07-15'),
(15, 'Brook', 'Lopez', 'C', 85, 282, '1988-04-01'),

(16, 'Donovan', 'Mitchell', 'SG', 75, 215, '1996-09-07'),
(17, 'Darius', 'Garland', 'PG', 73, 192, '2000-01-26'),
(18, 'Evan', 'Mobley', 'PF', 83, 215, '2001-06-18'),

(19, 'Tyrese', 'Haliburton', 'PG', 77, 185, '2000-02-29'),
(20, 'Pascal', 'Siakam', 'PF', 80, 245, '1994-04-02'),
(21, 'Myles', 'Turner', 'C', 83, 250, '1996-03-24'),

(22, 'Jimmy', 'Butler', 'SF', 79, 230, '1989-09-14'),
(23, 'Bam', 'Adebayo', 'C', 81, 255, '1997-07-18'),
(24, 'Tyler', 'Herro', 'SG', 77, 195, '2000-01-20'),

(25, 'Paolo', 'Banchero', 'PF', 82, 250, '2002-11-12'),
(26, 'Franz', 'Wagner', 'SF', 82, 220, '2001-08-27'),
(27, 'Jalen', 'Suggs', 'PG', 76, 205, '2001-06-03'),

(28, 'Trae', 'Young', 'PG', 73, 164, '1998-09-19'),
(29, 'Dejounte', 'Murray', 'SG', 77, 180, '1996-09-19'),
(30, 'Clint', 'Capela', 'C', 82, 256, '1994-05-18');


INSERT INTO Roster (roster_id, jersey_number, start_date, end_date, player_id, team_id, season_id, status_id)
VALUES
#2022-2023
(1, 0, '2022-10-01 00:00:00', NULL, 1, 1, 1, 1),
(2, 7, '2022-10-01 00:00:00', NULL, 2, 1, 1, 1),
(3, 9, '2022-10-01 00:00:00', NULL, 3, 1, 1, 1),
(4, 11, '2022-10-01 00:00:00', NULL, 4, 2, 1, 1),
(5, 3, '2022-10-01 00:00:00', NULL, 5, 2, 1, 1),
(6, 25, '2022-10-01 00:00:00', NULL, 6, 2, 1, 1),
(7, 21, '2022-10-01 00:00:00', NULL, 7, 3, 1, 1),
(8, 0, '2022-10-01 00:00:00', NULL, 8, 3, 1, 1),
(9, 8, '2022-10-01 00:00:00', NULL, 9, 3, 1, 1),
(10, 24, '2022-10-01 00:00:00', NULL, 10, 4, 1, 1),
(11, 33, '2022-10-01 00:00:00', NULL, 11, 4, 1, 1),
(12, 2, '2022-10-01 00:00:00', NULL, 12, 4, 1, 1),
(13, 34, '2022-10-01 00:00:00', NULL, 13, 5, 1, 1),
(14, 0, '2022-10-01 00:00:00', NULL, 14, 5, 1, 1),
(15, 11, '2022-10-01 00:00:00', NULL, 15, 5, 1, 1),
(16, 45, '2022-10-01 00:00:00', NULL, 16, 6, 1, 1),
(17, 10, '2022-10-01 00:00:00', NULL, 17, 6, 1, 1),
(18, 4, '2022-10-01 00:00:00', NULL, 18, 6, 1, 1),
(19, 0, '2022-10-01 00:00:00', NULL, 19, 7, 1, 1),
(20, 43, '2022-10-01 00:00:00', NULL, 20, 7, 1, 1),
(21, 33, '2022-10-01 00:00:00', NULL, 21, 7, 1, 1),
(22, 22, '2022-10-01 00:00:00', NULL, 22, 8, 1, 1),
(23, 13, '2022-10-01 00:00:00', NULL, 23, 8, 1, 1),
(24, 14, '2022-10-01 00:00:00', NULL, 24, 8, 1, 1),
(25, 5, '2022-10-01 00:00:00', NULL, 25, 9, 1, 1),
(26, 22, '2022-10-01 00:00:00', NULL, 26, 9, 1, 1),
(27, 4, '2022-10-01 00:00:00', NULL, 27, 9, 1, 1),
(28, 11, '2022-10-01 00:00:00', NULL, 28, 10, 1, 1),
(29, 5, '2022-10-01 00:00:00', NULL, 29, 10, 1, 1),
(30, 15, '2022-10-01 00:00:00', NULL, 30, 10, 1, 1),

#2023-2024
(31, 0, '2023-10-01 00:00:00', NULL, 1, 1, 2, 1),
(32, 7, '2023-10-01 00:00:00', NULL, 2, 1, 2, 1),
(33, 9, '2023-10-01 00:00:00', NULL, 3, 1, 2, 1),
(34, 11, '2023-10-01 00:00:00', NULL, 4, 2, 2, 1),
(35, 3, '2023-10-01 00:00:00', NULL, 5, 2, 2, 1),
(36, 25, '2023-10-01 00:00:00', NULL, 6, 2, 2, 1),
(37, 21, '2023-10-01 00:00:00', NULL, 7, 3, 2, 1),
(38, 0, '2023-10-01 00:00:00', NULL, 8, 3, 2, 1),
(39, 8, '2023-10-01 00:00:00', NULL, 9, 3, 2, 1),
(40, 24, '2023-10-01 00:00:00', NULL, 10, 4, 2, 1),
(41, 33, '2023-10-01 00:00:00', NULL, 11, 4, 2, 1),
(42, 2, '2023-10-01 00:00:00', NULL, 12, 4, 2, 1),
(43, 34, '2023-10-01 00:00:00', NULL, 13, 5, 2, 1),
(44, 0, '2023-10-01 00:00:00', NULL, 14, 5, 2, 1),
(45, 11, '2023-10-01 00:00:00', NULL, 15, 5, 2, 1),
(46, 45, '2023-10-01 00:00:00', NULL, 16, 6, 2, 1),
(47, 10, '2023-10-01 00:00:00', NULL, 17, 6, 2, 1),
(48, 4, '2023-10-01 00:00:00', NULL, 18, 6, 2, 1),
(49, 0, '2023-10-01 00:00:00', NULL, 19, 7, 2, 1),
(50, 43, '2023-10-01 00:00:00', NULL, 20, 7, 2, 1),
(51, 33, '2023-10-01 00:00:00', NULL, 21, 7, 2, 1),
(52, 22, '2023-10-01 00:00:00', NULL, 22, 8, 2, 1),
(53, 13, '2023-10-01 00:00:00', NULL, 23, 8, 2, 1),
(54, 14, '2023-10-01 00:00:00', NULL, 24, 8, 2, 1),
(55, 5, '2023-10-01 00:00:00', NULL, 25, 9, 2, 1),
(56, 22, '2023-10-01 00:00:00', NULL, 26, 9, 2, 1),
(57, 4, '2023-10-01 00:00:00', NULL, 27, 9, 2, 1),
(58, 11, '2023-10-01 00:00:00', NULL, 28, 10, 2, 1),
(59, 5, '2023-10-01 00:00:00', NULL, 29, 10, 2, 1),
(60, 15, '2023-10-01 00:00:00', NULL, 30, 10, 2, 1),

#2024-2025
(61, 0, '2024-10-01 00:00:00', NULL, 1, 1, 3, 1),
(62, 7, '2024-10-01 00:00:00', NULL, 2, 1, 3, 1),
(63, 9, '2024-10-01 00:00:00', NULL, 3, 1, 3, 1),
(64, 11, '2024-10-01 00:00:00', NULL, 4, 2, 3, 1),
(65, 3, '2024-10-01 00:00:00', NULL, 5, 2, 3, 1),
(66, 25, '2024-10-01 00:00:00', NULL, 6, 2, 3, 1),
(67, 21, '2024-10-01 00:00:00', NULL, 7, 3, 3, 1),
(68, 0, '2024-10-01 00:00:00', NULL, 8, 3, 3, 1),
(69, 8, '2024-10-01 00:00:00', NULL, 9, 3, 3, 1),
(70, 24, '2024-10-01 00:00:00', NULL, 10, 4, 3, 1),
(71, 33, '2024-10-01 00:00:00', NULL, 11, 4, 3, 1),
(72, 2, '2024-10-01 00:00:00', NULL, 12, 4, 3, 1),
(73, 34, '2024-10-01 00:00:00', NULL, 13, 5, 3, 1),
(74, 0, '2024-10-01 00:00:00', NULL, 14, 5, 3, 1),
(75, 11, '2024-10-01 00:00:00', NULL, 15, 5, 3, 1),
(76, 45, '2024-10-01 00:00:00', NULL, 16, 6, 3, 1),
(77, 10, '2024-10-01 00:00:00', NULL, 17, 6, 3, 1),
(78, 4, '2024-10-01 00:00:00', NULL, 18, 6, 3, 1),
(79, 0, '2024-10-01 00:00:00', NULL, 19, 7, 3, 1),
(80, 43, '2024-10-01 00:00:00', NULL, 20, 7, 3, 1),
(81, 33, '2024-10-01 00:00:00', NULL, 21, 7, 3, 1),
(82, 22, '2024-10-01 00:00:00', NULL, 22, 8, 3, 1),
(83, 13, '2024-10-01 00:00:00', NULL, 23, 8, 3, 1),
(84, 14, '2024-10-01 00:00:00', NULL, 24, 8, 3, 1),
(85, 5, '2024-10-01 00:00:00', NULL, 25, 9, 3, 1),
(86, 22, '2024-10-01 00:00:00', NULL, 26, 9, 3, 1),
(87, 4, '2024-10-01 00:00:00', NULL, 27, 9, 3, 1),
(88, 11, '2024-10-01 00:00:00', NULL, 28, 10, 3, 1),
(89, 5, '2024-10-01 00:00:00', NULL, 29, 10, 3, 1),
(90, 15, '2024-10-01 00:00:00', NULL, 30, 10, 3, 1);

INSERT INTO Game
(game_id, game_date, home_score, away_score, arena_id, season_id, game_type_id, home_team_id, away_team_id)
VALUES
#2022-2023 regular season
(1, '2022-10-20 19:00:00', 112, 105, 1, 1, 1, 1, 2),
(2, '2022-10-22 19:30:00', 108, 111, 5, 1, 1, 5, 6),
(3, '2022-10-25 19:00:00', 115, 109, 8, 1, 1, 8, 9),
(4, '2022-11-01 19:30:00', 120, 114, 3, 1, 1, 3, 4),
(5, '2022-11-05 18:00:00', 117, 121, 7, 1, 1, 7, 10),
(6, '2022-11-08 19:00:00', 110, 104, 2, 1, 1, 2, 1),
#2022-2023 postseason
(7, '2023-04-20 19:00:00', 118, 110, 1, 1, 2, 1, 8),
(8, '2023-04-22 20:00:00', 107, 101, 5, 1, 2, 5, 6),

#2023-2024 regular season
(9, '2023-10-18 19:00:00', 119, 113, 1, 2, 1, 1, 3),
(10, '2023-10-21 19:30:00', 104, 99, 2, 2, 1, 2, 4),
(11, '2023-10-24 19:00:00', 122, 116, 6, 2, 1, 6, 7),
(12, '2023-10-28 19:30:00', 108, 111, 9, 2, 1, 9, 8),
(13, '2023-11-02 19:00:00', 125, 119, 10, 2, 1, 10, 5),
(14, '2023-11-06 19:30:00', 114, 109, 4, 2, 1, 4, 3),
#2023-2024 postseason
(15, '2024-04-18 19:00:00', 116, 112, 1, 2, 2, 1, 5),
(16, '2024-04-21 19:30:00', 110, 106, 8, 2, 2, 8, 9),

#2024-2025 regular season
(17, '2024-10-19 19:00:00', 113, 108, 1, 3, 1, 1, 6),
(18, '2024-10-22 19:30:00', 107, 115, 2, 3, 1, 2, 7),
(19, '2024-10-26 19:00:00', 121, 117, 3, 3, 1, 3, 8),
(20, '2024-10-30 19:30:00', 109, 103, 5, 3, 1, 5, 10),
(21, '2024-11-03 18:00:00', 111, 105, 9, 3, 1, 9, 4),
(22, '2024-11-08 19:00:00', 118, 120, 6, 3, 1, 6, 5),
#2024-2025 postseason
(23, '2025-04-19 19:00:00', 120, 114, 1, 3, 2, 1, 2),
(24, '2025-04-22 20:00:00', 112, 109, 5, 3, 2, 5, 8);

INSERT INTO PlayerGameStats
(player_id, game_id, team_id, points, rebounds, assists, minutes_played, steals, blocks, turnovers)
VALUES
#Game 1: BOS vs NYK
(1, 1, 1, 31, 8, 5, 37, 2, 1, 3),
(2, 1, 1, 26, 6, 4, 35, 1, 0, 2),
(3, 1, 1, 18, 4, 7, 34, 3, 1, 1),
(4, 1, 2, 28, 3, 8, 36, 1, 0, 2),
(5, 1, 2, 17, 9, 5, 35, 2, 0, 2),
(6, 1, 2, 20, 5, 3, 34, 1, 1, 3),

#Game 2: MIL vs CLE
(13, 2, 5, 34, 12, 6, 38, 1, 2, 4),
(14, 2, 5, 25, 4, 9, 36, 2, 0, 3),
(15, 2, 5, 15, 10, 2, 31, 0, 3, 1),
(16, 2, 6, 29, 5, 4, 37, 2, 0, 2),
(17, 2, 6, 21, 3, 8, 35, 1, 0, 2),
(18, 2, 6, 17, 11, 3, 33, 1, 2, 2),

#Game 3: MIA vs ORL
(22, 3, 8, 27, 6, 5, 36, 2, 1, 2),
(23, 3, 8, 22, 12, 4, 35, 1, 1, 3),
(24, 3, 8, 20, 4, 6, 34, 0, 0, 2),
(25, 3, 9, 30, 9, 5, 37, 1, 1, 2),
(26, 3, 9, 19, 7, 4, 35, 1, 0, 2),
(27, 3, 9, 18, 5, 7, 34, 2, 0, 3),

#Game 4: PHI vs BKN
(7, 4, 3, 33, 13, 4, 38, 1, 3, 4),
(8, 4, 3, 24, 3, 7, 36, 2, 0, 2),
(9, 4, 3, 21, 6, 5, 34, 1, 1, 2),
(10, 4, 4, 29, 4, 3, 37, 1, 0, 3),
(11, 4, 4, 18, 11, 2, 34, 1, 2, 2),
(12, 4, 4, 20, 5, 4, 33, 2, 0, 2),

#Game 5: IND vs ATL
(19, 5, 7, 26, 4, 11, 37, 2, 0, 3),
(20, 5, 7, 23, 8, 4, 35, 1, 1, 2),
(21, 5, 7, 19, 10, 2, 33, 0, 2, 2),
(28, 5, 10, 32, 3, 10, 38, 2, 0, 4),
(29, 5, 10, 24, 6, 5, 36, 1, 1, 2),
(30, 5, 10, 18, 12, 1, 34, 1, 2, 2),

#Game 6: NYK vs BOS
(4, 6, 2, 25, 4, 8, 36, 1, 0, 2),
(5, 6, 2, 16, 10, 4, 35, 2, 0, 2),
(6, 6, 2, 18, 6, 3, 34, 1, 0, 2),
(1, 6, 1, 29, 9, 4, 37, 2, 1, 3),
(2, 6, 1, 22, 5, 3, 35, 1, 0, 2),
(3, 6, 1, 17, 4, 8, 34, 3, 0, 1),

#Game 7: BOS vs MIA postseason
(1, 7, 1, 35, 10, 5, 39, 2, 1, 3),
(2, 7, 1, 24, 6, 4, 37, 1, 0, 2),
(3, 7, 1, 19, 5, 7, 36, 2, 1, 1),
(22, 7, 8, 28, 7, 4, 38, 2, 1, 2),
(23, 7, 8, 21, 11, 3, 36, 1, 1, 2),
(24, 7, 8, 17, 4, 5, 35, 0, 0, 3),

#Game 8: MIL vs CLE postseason
(13, 8, 5, 30, 11, 7, 39, 1, 2, 4),
(14, 8, 5, 23, 5, 8, 37, 1, 0, 2),
(15, 8, 5, 16, 9, 2, 33, 0, 2, 1),
(16, 8, 6, 27, 4, 4, 38, 2, 0, 2),
(17, 8, 6, 19, 3, 7, 35, 1, 0, 2),
(18, 8, 6, 14, 10, 2, 34, 1, 2, 2),

#Game 9: BOS vs PHI
(1, 9, 1, 33, 9, 5, 38, 2, 1, 2),
(2, 9, 1, 25, 6, 4, 36, 1, 0, 2),
(3, 9, 1, 20, 4, 8, 35, 2, 1, 1),
(7, 9, 3, 31, 12, 3, 38, 1, 2, 4),
(8, 9, 3, 26, 4, 6, 36, 2, 0, 2),
(9, 9, 3, 18, 5, 4, 34, 1, 0, 3),

#Game 10: NYK vs BKN
(4, 10, 2, 27, 3, 9, 37, 1, 0, 2),
(5, 10, 2, 15, 8, 5, 35, 2, 0, 2),
(6, 10, 2, 19, 5, 3, 34, 1, 1, 2),
(10, 10, 4, 24, 4, 2, 36, 1, 0, 3),
(11, 10, 4, 16, 10, 2, 34, 1, 1, 2),
(12, 10, 4, 17, 6, 4, 33, 2, 0, 2),

#Game 11: CLE vs IND
(16, 11, 6, 30, 5, 5, 38, 2, 0, 2),
(17, 11, 6, 20, 3, 9, 36, 1, 0, 2),
(18, 11, 6, 18, 10, 3, 34, 1, 2, 2),
(19, 11, 7, 25, 4, 10, 37, 2, 0, 3),
(20, 11, 7, 22, 8, 5, 35, 1, 1, 2),
(21, 11, 7, 17, 9, 2, 33, 0, 2, 2),

#Game 12: ORL vs MIA
(25, 12, 9, 29, 8, 4, 37, 1, 1, 2),
(26, 12, 9, 20, 6, 3, 35, 1, 0, 2),
(27, 12, 9, 17, 4, 8, 34, 2, 0, 3),
(22, 12, 8, 26, 6, 4, 36, 2, 1, 2),
(23, 12, 8, 23, 12, 3, 35, 1, 1, 3),
(24, 12, 8, 21, 5, 5, 34, 1, 0, 2),

#Game 13: ATL vs MIL
(28, 13, 10, 31, 3, 11, 38, 1, 0, 4),
(29, 13, 10, 23, 5, 6, 36, 2, 0, 2),
(30, 13, 10, 18, 11, 1, 34, 1, 1, 2),
(13, 13, 5, 35, 13, 5, 39, 1, 2, 3),
(14, 13, 5, 24, 4, 8, 36, 2, 0, 2),
(15, 13, 5, 17, 9, 2, 32, 0, 2, 1),

#Game 14: BKN vs PHI
(10, 14, 4, 28, 4, 3, 37, 1, 0, 3),
(11, 14, 4, 17, 10, 2, 34, 1, 2, 2),
(12, 14, 4, 19, 6, 5, 34, 2, 0, 2),
(7, 14, 3, 32, 11, 4, 38, 1, 3, 4),
(8, 14, 3, 25, 3, 7, 36, 2, 0, 2),
(9, 14, 3, 20, 6, 3, 35, 1, 1, 2),

#Game 15: BOS vs MIL postseason
(1, 15, 1, 34, 10, 4, 39, 2, 1, 3),
(2, 15, 1, 23, 6, 5, 37, 1, 0, 2),
(3, 15, 1, 18, 4, 7, 36, 2, 1, 1),
(13, 15, 5, 31, 12, 6, 39, 1, 2, 4),
(14, 15, 5, 22, 4, 9, 37, 2, 0, 2),
(15, 15, 5, 16, 8, 2, 33, 0, 2, 1),

#Game 16: MIA vs ORL postseason
(22, 16, 8, 29, 7, 5, 38, 2, 1, 2),
(23, 16, 8, 22, 11, 3, 36, 1, 1, 2),
(24, 16, 8, 18, 4, 6, 35, 1, 0, 2),
(25, 16, 9, 27, 8, 4, 37, 1, 1, 2),
(26, 16, 9, 18, 7, 3, 35, 1, 0, 2),
(27, 16, 9, 16, 5, 7, 34, 2, 0, 3),

#Game 17: BOS vs CLE
(1, 17, 1, 30, 8, 5, 37, 2, 1, 3),
(2, 17, 1, 24, 5, 4, 35, 1, 0, 2),
(3, 17, 1, 19, 4, 8, 34, 3, 0, 1),
(16, 17, 6, 28, 4, 4, 37, 2, 0, 2),
(17, 17, 6, 20, 3, 7, 35, 1, 0, 2),
(18, 17, 6, 16, 10, 3, 34, 1, 2, 2),

#Game 18: NYK vs IND
(4, 18, 2, 26, 3, 8, 36, 1, 0, 2),
(5, 18, 2, 18, 8, 5, 35, 2, 0, 2),
(6, 18, 2, 17, 5, 3, 34, 1, 0, 2),
(19, 18, 7, 29, 4, 10, 37, 2, 0, 3),
(20, 18, 7, 24, 7, 4, 35, 1, 1, 2),
(21, 18, 7, 19, 9, 2, 33, 0, 2, 2),

#Game 19: PHI vs MIA
(7, 19, 3, 34, 12, 4, 38, 1, 2, 4),
(8, 19, 3, 26, 3, 7, 36, 2, 0, 2),
(9, 19, 3, 19, 6, 4, 35, 1, 0, 2),
(22, 19, 8, 28, 7, 5, 38, 2, 1, 2),
(23, 19, 8, 21, 11, 3, 36, 1, 1, 3),
(24, 19, 8, 20, 4, 5, 35, 0, 0, 2),

#Game 20: MIL vs ATL
(13, 20, 5, 33, 12, 5, 38, 1, 2, 3),
(14, 20, 5, 24, 4, 8, 36, 2, 0, 2),
(15, 20, 5, 18, 9, 2, 32, 0, 2, 1),
(28, 20, 10, 27, 3, 10, 37, 2, 0, 4),
(29, 20, 10, 21, 5, 5, 36, 1, 0, 2),
(30, 20, 10, 16, 11, 1, 34, 1, 1, 2),

#Game 21: ORL vs BKN
(25, 21, 9, 28, 8, 4, 37, 1, 1, 2),
(26, 21, 9, 19, 6, 3, 35, 1, 0, 2),
(27, 21, 9, 17, 5, 7, 34, 2, 0, 3),
(10, 21, 4, 24, 4, 2, 36, 1, 0, 3),
(11, 21, 4, 17, 9, 2, 34, 1, 1, 2),
(12, 21, 4, 18, 5, 4, 33, 2, 0, 2),

#Game 22: CLE vs MIL
(16, 22, 6, 31, 5, 4, 38, 2, 0, 2),
(17, 22, 6, 21, 3, 8, 36, 1, 0, 2),
(18, 22, 6, 18, 10, 3, 34, 1, 2, 2),
(13, 22, 5, 35, 13, 6, 39, 1, 2, 4),
(14, 22, 5, 26, 4, 9, 37, 2, 0, 2),
(15, 22, 5, 19, 8, 2, 33, 0, 2, 1),

#Game 23: BOS vs NYK postseason
(1, 23, 1, 36, 10, 5, 39, 2, 1, 3),
(2, 23, 1, 25, 6, 4, 37, 1, 0, 2),
(3, 23, 1, 20, 4, 8, 36, 3, 1, 1),
(4, 23, 2, 29, 3, 9, 38, 1, 0, 2),
(5, 23, 2, 18, 8, 5, 36, 2, 0, 2),
(6, 23, 2, 17, 5, 3, 35, 1, 0, 2),

#Game 24: MIL vs MIA postseason
(13, 24, 5, 32, 12, 6, 39, 1, 2, 3),
(14, 24, 5, 23, 4, 8, 37, 2, 0, 2),
(15, 24, 5, 17, 9, 2, 33, 0, 2, 1),
(22, 24, 8, 27, 7, 4, 38, 2, 1, 2),
(23, 24, 8, 22, 11, 3, 36, 1, 1, 3),
(24, 24, 8, 19, 4, 6, 35, 0, 0, 2);



