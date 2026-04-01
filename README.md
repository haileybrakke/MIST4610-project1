# MIST4610-project1

# Group

Name: Group 9

Members:

Hailey Brakke (https://github.com/haileybrakke/MIST4610-project1) 

Will Federer (https://github.com/willfederer10/MIST4610-project1))

Summer Sayedzada (https://github.com/ss83771/project1)

Tony Jimenez (https://github.com/tonyj010/MIST4610)

Ja’Khiyan Dowdy (https://github.com/jakhiyan0219-hub/Project-1.git)

# Scenario Description

Our client is the Operations Manager for the NBA Eastern Conference. The Eastern Conference league office supports reporting and coordination across all 15 Eastern Conference teams. The office needs to track team participation, player rosters, game schedules and results, coaching assignments, and player performance throughout the season. Currently, this information is stored across multiple spreadsheets and public sources, making it difficult to monitor roster changes over time, analyze team and player performance, and generate reliable operational reports. This project creates a centralized relational database to manage publicly available basketball operations data and improve reporting efficiency. 

# Data Model

Model Explanation: The model represents the NBA league structure by linking conferences, divisions, and teams together. Each team belongs to a division, and each division belongs to a conference. The model also tracks player participation through the Roster table, which connects players to teams and seasons, allowing players to move between teams over time while maintaining historical records.

Game data includes the date, location (arena), season, type (regular season or postseason), and competing teams (home and away). Each game is linked to one arena, one season, and one game type, while teams can participate in many games. 

Players are stored in the Player table, which contains demographic and physical attributes. Because a player can appear on multiple rosters over time (across seasons or teams), a many-to-many relationship exists between Player and Roster. This relationship is resolved through the Player_has_Roster junction table.

Player performance is recorded in the PlayerGameStats table, which connects players to games and stores basic statistics such as points. Coaching history is tracked using the TeamCoachAssignment table, which links coaches to teams over time.

This database supports queries looking to answer questions involving team structure, player movement, game outcomes, and individual performance across seasons. However, it is not capable of answering questions related to detailed play-by-play data, advanced statistics, financial information, or teams beyond the Eastern Conference.
<img width="969" height="703" alt="Screenshot 2026-03-31 at 5 11 38 PM" src="https://github.com/user-attachments/assets/8085349e-c4af-4855-9f63-441fe9cba81f" />


# Data Dictionary
<img width="688" height="312" alt="Screenshot 2026-03-31 at 10 05 44 AM" src="https://github.com/user-attachments/assets/20c4005c-06a2-43de-a6a2-0857462071ed" />
<img width="665" height="461" alt="Screenshot 2026-03-31 at 10 06 34 AM" src="https://github.com/user-attachments/assets/b8fc697e-ed83-4f9b-84b9-7838e69db537" />
<img width="673" height="521" alt="Screenshot 2026-03-31 at 10 07 15 AM" src="https://github.com/user-attachments/assets/19f663c5-8468-460f-8151-50340317e04e" />
<img width="673" height="424" alt="Screenshot 2026-03-31 at 10 14 17 AM" src="https://github.com/user-attachments/assets/23cbf110-55a9-4932-8c83-11fec2a487a7" />
<img width="658" height="436" alt="Screenshot 2026-03-31 at 10 15 48 AM" src="https://github.com/user-attachments/assets/e422d373-6000-4520-b1f6-7a9185e6aa87" />
<img width="648" height="268" alt="Screenshot 2026-03-31 at 10 17 05 AM" src="https://github.com/user-attachments/assets/80204598-61bf-4714-af20-8ffe904341dc" />
<img width="587" height="640" alt="Screenshot 2026-03-31 at 10 18 25 AM" src="https://github.com/user-attachments/assets/be20fb67-37d3-4364-b03f-da07195e0d09" />
<img width="594" height="292" alt="Screenshot 2026-03-31 at 10 44 35 AM" src="https://github.com/user-attachments/assets/de70b645-83c7-4b77-9cdc-b91eaa8e34f4" />
<img width="660" height="543" alt="Screenshot 2026-03-31 at 5 02 45 PM" src="https://github.com/user-attachments/assets/1e6388fb-f5fa-4750-81c5-3d194697dd9d" />
<img width="596" height="320" alt="Screenshot 2026-03-31 at 10 47 30 AM" src="https://github.com/user-attachments/assets/8d9a8c83-46a4-43da-a553-7f1b66cca067" />
<img width="628" height="644" alt="Screenshot 2026-03-31 at 10 48 56 AM" src="https://github.com/user-attachments/assets/f6514f91-a900-4beb-8189-3095e9e09acb" />
<img width="637" height="332" alt="Screenshot 2026-03-31 at 10 49 16 AM" src="https://github.com/user-attachments/assets/ee330fbe-fc41-4ff8-b43a-9b34c90ea64c" />
<img width="656" height="295" alt="Screenshot 2026-03-31 at 5 08 04 PM" src="https://github.com/user-attachments/assets/e3b0a50b-eb45-4b9c-a106-239f67f857e6" />
<img width="687" height="454" alt="Screenshot 2026-03-31 at 5 10 13 PM" src="https://github.com/user-attachments/assets/1bd5c50b-bb31-4fa0-8f53-a0df9aec73ab" />





# Query Matrix 
<img width="627" height="401" alt="Screenshot 2026-03-31 at 10 04 02 PM" src="https://github.com/user-attachments/assets/111dae27-f433-4ef2-972e-871a316d0c44" />

# Queries
Query 1: Which coach is assigned to which team?
Justification: Query 1 joins the TeamCoachAssignment, Team, and Coach tables to show which coach is assigned to each team. It focuses only on names to provide a simple and easy-to-read list of coach–team pairings.
<img width="983" height="459" alt="Screenshot 2026-03-31 at 11 12 25 AM" src="https://github.com/user-attachments/assets/ebb3017f-ea44-4194-8c6c-7b33c8d4dd35" />

Query 2: How many players are on each team for each season?
Justification: Query 2 joins the Team and Season tables to show the amount of players that are on each team for each season. It reveals how many players are active and participated in the season games.

<img width="469" height="710" alt="Screenshot 2026-03-31 at 12 01 12 PM" src="https://github.com/user-attachments/assets/92536328-de82-4fcc-b82d-325233471151" />


Query 3: Retrieve all games played on a certain date
Justification: With this query if our client needs information about a game played on a specific date we can easily find that. This query shows the home team, away team, score, and date of the game played.


<img width="566" height="386" alt="FXED" src="https://github.com/user-attachments/assets/6dd67c89-7eaf-4a9f-b0a6-8d580f030d1e" />




<img width="608" height="90" alt="FIXED2" src="https://github.com/user-attachments/assets/8ccd2e9b-40a9-4c92-bcfa-6474a636bd73" />



Query 4: Based off points scored, show all home team wins per season per team.
Justification: This query allows our client to see which teams excel on their home court and lets the client make comparisons and judgements concerning team performance.
<img width="1330" height="159" alt="image" src="https://github.com/user-attachments/assets/00235e6b-f153-4d38-aac0-1f0e203cb455" />
<img width="786" height="603" alt="image" src="https://github.com/user-attachments/assets/6b5d33ec-155f-4a09-9afa-65b042dc03e2" />
<img width="768" height="151" alt="image" src="https://github.com/user-attachments/assets/ac976427-f60f-4cb9-ab34-097e70fe6311" />




Query 5: Who is the highest scoring player in each game?
Justification: Query 5 allows teams to see who preformed the best in a given game. This can be used by managers to identify the leauge's top performing players, which could be used for all-star selection and MVP voting.

<img width="1106" height="382" alt="Screenshot 2026-03-31 180527" src="https://github.com/user-attachments/assets/4ecc9900-d648-4ab7-bf44-7afd4dfcb267" />

Query 6: List players who played on multiple rosters in the same season.
Justification: Query 6 shows players who played on two or more different rosters in the same season. Managers can use this data to track trades between teams and free agency pickups. 

<img width="1026" height="298" alt="Screenshot 2026-03-31 215528" src="https://github.com/user-attachments/assets/2a6e3319-14fb-4de5-b661-b57d8a2c2e7c" />



Query 7: Compare highest scoring teams between divisions
Justification: Query 7 lets the client examine which team among all divisions is doing the best based off highest scored points. If the client was interested in analyzing team performance this could be useful information.
<img width="1556" height="251" alt="image" src="https://github.com/user-attachments/assets/c617c844-28f5-408a-b066-16c0e32653b8" />
<img width="459" height="158" alt="image" src="https://github.com/user-attachments/assets/d62ac974-f59a-40d6-bd32-a7c8bddf570d" />


Query 8: Compare two different player's stats for a season 
Justification: Query 8 allows our client to be able to compare two players statisitics in their games played throughout the seasons we have in our database. If our clients objective is to find data about players before a matchup and how they compare between two players they are able to do that.


<img width="432" height="338" alt="COMPARINGTWOQUERY" src="https://github.com/user-attachments/assets/ea9a33a4-14ca-4519-a365-fa6c1932b052" />


<img width="495" height="72" alt="COMPARINGTWO" src="https://github.com/user-attachments/assets/af0aa8b5-40e1-41cd-aa2b-1749e42e9738" />


Query 9: Which players score above average points accross all documented seasons?
Justification: Query 9 allows the Operations Manager to see what players have been consistently scoring in the top 50% across all seasons. This reveals which players are top performers when comparing them against all teams.

<img width="739" height="526" alt="Screenshot 2026-03-31 at 4 26 33 PM" src="https://github.com/user-attachments/assets/d8eb6b55-8a9a-48b9-94f8-a51568565bf9" />

Query 10: Which players only played between 1-5 games in the most recent season?
Justification: Query 10 allows the Operations Manager to see what players have played between 1-5 games in the past season. This reveals what players are contributing the least overall in the league, which is valuable information when considering what players teams wish to keep on their roster in the future.

<img width="432" height="769" alt="Screenshot 2026-03-31 at 4 31 20 PM" src="https://github.com/user-attachments/assets/46dd14db-a585-4b75-912d-eaf84535e8ec" />



# Database Information:

Name: al_Group_21482_G9

Additional information: Each query listed above is marked in the database using stored procedures which can be called using the following format: CALL TP_Q1();
