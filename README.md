# Fifa23

SQL assignment FIFA23 by Hampus, Tobias &amp; Tommi

### Query Examples

#### List all games today

> > > select \* from Matches
> > > where date = "(mm/dd/yyyy)"

#### List a teams matches and results

> > > SELECT home.name as home_team, m.results , away.name as away_team
> > > FROM matches as m
> > > INNER join teams as home
> > > on home.id = m.home_team_id
> > > INNER JOIN teams as away
> > > on away.id = m.away_team_id
> > > WHERE home.id = 2 or away.id = 2;
