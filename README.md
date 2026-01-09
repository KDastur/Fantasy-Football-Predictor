# Fantasy-Football-Predictor
A Python-based prediction model that stores, and evaluates player performance by weighting historical averages, consistency, and opponent matchups to estimate future scores.

```python
while True:
    ans = input("Add player (a), Delete player (d), Compare players (c), or View team (v): ").lower()
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
