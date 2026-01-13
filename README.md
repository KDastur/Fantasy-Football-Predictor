# Fantasy-Football-Predictor
### A Python-based prediction model that stores, and evaluates player performance by weighting historical averages, consistency, and opponent matchups to estimate future scores.

How To Use: this program starts by asking the user if they would like to add a player, delete a player, or view all players on his fantasy football team. If the user adds a player, he will be prompted to provide information such as player name, position, their matchup strength for that specific week, their average points per game up to that week, and an subjective rating of the player's consistency. This will then output the predicted score for that player next week, as well as a likely range that the player will fall into, with the upper bound and lower bound being determined by the player's consistency. this stores the player's information and predicted score in a list. it then prompts you to add/delete a player, or view team. deleting a player simply provides a numbered list of all players, and lets the user choose one to delete from the list. finally if the user chooses to view the team, it provides the full list of all the players entered, and allows the user to compare each player's prediction, and range of cores to pick their best team. (Note: after entering real information for 14 players for a week of football, 12/14 players has a score in their predicted range)

```python
while True:
    ans = input("Add player (a), Delete player (d), or View team (v): ").lower()
    if ans == "a":
        b = input("Player first name: ").capitalize()
        c = input("Player last name: ").capitalize()
        a = input("Player position: ").capitalize()
        d = input("Player team: ").capitalize()
        f = input("Player matchup strength (number): ")
        try:
            f = int(f)
        except:
            print("Player matchup strength must be a number")
            continue
        g = input("Player's average fantasy score: ")
        try:
            g = float(g)
        except:
            print("Player's average score must be a number")
        matchup = f/100 + .84
        projected = round(g * matchup, 2)
        h = input("Player consistency score (1-10, 10 being the best): ")
        try:
            h = int(h)
        except:
            print("Player consistency score must be a number")
            continue
        consist = (h-10) * -0.05
        top = 1.2 + consist
        bottom = .8 - consist
        pro_max = round(projected * top, 2)
        pro_min = round(projected * bottom, 2)
        projected = str(projected)
        pro_min = str(pro_min)
        pro_max = str(pro_max)
        with open("fantasyteam.txt", 'a') as team:
                team.write(f"\n{b} {c}, {a}, {d} \nLower:    Proj:     Upper:    \n{pro_min}     {projected}     {pro_max}\n\n")
        with open("fantasyteam.txt", 'r') as team:
            for line in team:
                line = line.strip()
                print(line)
    if ans == "d":
        with open("fantasyteam.txt", 'r') as team:
            lines = team.readlines()
        entries = []
        current = []
        for line in lines:
            line = line.strip()
            if line:
                current.append(line)
            else:
                if current:
                    entries.append(current)
                    current = []
        if current:
            entries.append(current)
        for i, players in enumerate(entries, start=1):
            print(f'{i}. {players[0]}')
        delete = input("Which player would you like to delete: ")
        try:
            delete = int(delete) - 1
            del entries[delete]
        except:
            print("invalid response")
            continue
        with open('fantasyteam.txt', 'w') as team:
            for players in entries:
                for line in players:
                    team.write(f"\n{line}")
                team.write("\n\n")
    if ans == "v":
        with open("fantasyteam.txt", 'r') as team:
            for line in team:
                line = line.strip()
                print(line)
        break
``` 
