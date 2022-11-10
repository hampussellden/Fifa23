# Fifa23

SQL assignment FIFA23 by Hampus, Tobias &amp; Tommi

### Query Examples

#### List all games today

> select \* from Matches
> where date = "(mm/dd/yyyy)"

#### List a teams matches and results

> SELECT home.name as home_team, m.results , away.name as away_team
> FROM matches as m
> INNER join teams as home
> on home.id = m.home_team_id
> INNER JOIN teams as away
> on away.id = m.away_team_id
> WHERE home.id = 2 or away.id = 2;

#### List a group table with teams, wins, draws, losses, goal-diff and total points

> Select UPPER(g.name) as "group", t.name as "team", g.win, g.loss, g.draw, g.goals_for, g.goals_against, g.total_points from groups as g
> inner JOIN teams as t
> on t.id = g.team_id
> where g.name = "a";

#### List top 10 players by goals, then by assists

> SELECT first_name, last_name, goals, assists
> FROM players
> ORDER BY goals DESC, assists DESC
> LIMIT 10;

#### list all players that are unavailable because of cards

> select \* from players
> where red_cards >= 1 or yellow_cards >=2;

#### List a teams roster with players and coach, goals, assists, shots and desciplinary

> Select teams.name, players.first_name, players.last_name, coaches.first_name as coach_fname, coaches.last_name as coach_lname,players.goals, players.assists, players.red_cards, players.yellow_cards, players.attempts from players
> inner join coaches
> on players.team_id = coaches.team_id
> inner join teams
> on teams.id = players.team_id
> where teams.name = "Sweden";

#### Detailed info about a finished game

##### Teams, players, goals, disciplinary, referee, venue, date, multiple players are often involved in a event

###### Show a good overview of the game, not including player names

> select distinct home.name as home_team, matches.results , away.name as away_team, venues.name as venue, referees.referee_team_id, matches.date from match_events
> inner join matches
> on matches.id = match_events.match_id
> inner join venues
> on matches.venue_id = venues.id
> inner join referees
> on matches.referee_id = referees.referee_team_id
> INNER join teams as home
> on home.id = matches.home_team_id
> INNER JOIN teams as away
> on away.id = matches.away_team_id
> where matches.id = 64;

###### Shows a timeline of the games primary events, the last name of the affected player(s), the venue, referee and date

> SELECT
> me.match_id AS match,
> me.time_regular,
> me.time_extra as et,
> me.type,
> scorer.last_name AS scorer,
> assist.last_name AS assistant,
> teams.name as team_name,
> cards.last_name as disciplinant,
> sub_in.last_name,
> sub_out.last_name,
> venues.name,
> referees.first_name as ref_FName, referees.last_name as ref_LName,
> matches.date
> FROM
> match_events AS me
> left JOIN players AS scorer ON scorer.id = me.scorer_id
> left JOIN players AS assist ON assist.id = me.assist_id
> left join players as cards on cards.id = me.yellow_card_id
> left join players as sub_in on sub_in.id = me.sub_in_id
> left join players as sub_out on sub_out.id = me.sub_out_id
> left JOIN teams ON teams.id = scorer.team_id
> left JOIN matches ON me.match_id = matches.id
> left join venues on matches.venue_id = venues.id
> left join referees on referees.referee_team_id = matches.referee_id
> WHERE match_id = 64
> group by me.time_regular;

#### All scores should contain info if it is after regular time (90+ minutes), after extra-time or after extra-time and penalty shoot-out

###### First half goals in final

> SELECT COUNT(type)
> FROM match_events
> WHERE type = "goal"
> AND match_id = 64
> AND time_regular < 46;

###### 2nd over time half goals in final

> SELECT COUNT(type)
> FROM match_events
> WHERE type = "goal"
> AND match_id = 61
> AND time_regular >= 106
> AND time_regular <= 120;

###### How many goals were made in the shootout, if you want to see missed penalties change shooutout goal to = 0

> SELECT COUNT(type)
> FROM match_events
> WHERE type = "shootout attempt"
> AND shootout_goal = 1
> AND match_id = 56
> AND time_regular = 120;
