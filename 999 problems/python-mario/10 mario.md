# mario.py

![](mario.png)

In deze opdracht ga je de mario opdracht van CS50 maken in Python.

## Gebruik

	python mario.py
	Height: 3
	  ##
	 ###
	####


	python mario.py
	Height: -1
	Height: 24
	Height: 2
	 ##
	###

## Specificatie

* Schrijf een programma genaamd `mario.py` dat de piramide van mario maakt gebruikmakend van hashes (`#`) voor blokken.
* De gebruiker van het programma mag zelf de hoogte van de pyramide specificeren.
* De hoogte van de pyramide mag niet groter dan 23 blokken hoog zijn, en niet kleiner dan 0. Wordt er een andere waarde ingevuld, dan moet je de gebruiker opnieuw om invoer vragen.
* Je mag aannemen dat de gebruiker enkel integers invoert.

## Tips

* Python scheidt print output normaal gesproken automatisch met een spatie. Wil je dat niet gebruik dan het extra `sep` argument als volgt: `print("foo", "bar", sep="")`.
* Python is handig met strings, zo kun je strings vermenigvuldigen: `"a" * 3`

## Testen

	checkpy mario
