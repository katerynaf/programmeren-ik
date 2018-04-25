# Sudoku Solver
Behalve programma's ontwikkelen voor eindgebruikers kunnen we programmeren ook inzetten om problemen / puzzels op te lossen. Een klassiek voorbeeld hiervan is een sudoku solver.

Sudoku's zijn je waarschijnlijk wel bekend. Ze zien er zo uit:

![](sudoku_example.png)

Een grid met 9x9 vakjes. In elk vakje moet uiteindelijk een getal tussen de 1 t/m 9 komen te staan. Maar, er zijn drie restricties:

* elke horizontale rij mag maar één van elk getal bevatten
* elke verticale kolom mag maar één van elk getal bevatten
* elk 3x3 blok mag maar één van elk getal bevatten (zie ook de donkere lijnen op het grid)

## What to do
* Implementeer een regelgebaseerde solver
* Implementeer een iteratieve DFS solver
* Implementeer een recursieve DFS solver
* (Optioneel) Implementeer een interatieve DFS solver met Python generators
* Genereer "proper sudoku's"

## Om te beginnen
Bij deze opdracht leveren we data & code mee. Deze kun je [hier downloaden](https://github.com/Jelleas/sudoku/archive/master.zip)

Je vindt hier twee mapjes: `easy` en `hard`. Met daarin, je raadt het al, makkelijke en moeilijke sudoku's. In het zelfverzonnen sudoku bestandstype. Een .sudoku ziet er zo uit:

    7,9,0,0,0,0,3,0,1
    0,0,0,0,0,6,9,0,0
    8,0,0,0,3,0,0,7,6
    0,0,0,0,0,5,0,0,2
    0,0,5,4,1,8,7,0,0
    4,0,0,7,0,0,0,0,0
    6,1,0,0,9,0,0,0,8
    0,0,2,3,0,0,0,0,0
    0,0,9,0,0,0,0,5,4

Waarin er op elke regel 9 cijfers staan. Een 0 staat voor een leeg vakje, en elk ander cijfer voor een ingevuld vakje met dat cijfer.

Ook vind je het bestand `solver.py`. Hierin ga jij straks aan de slag.

# Regelgebaseerde solver
Er zijn ontzettend [veel strategieën](http://www.sudokudragon.com/sudokustrategy.htm) om een sudoku puzzel op te lossen. Een simpele aanpak is via  de "Single possibility rule" die luidt als volgt: Als er maar één mogelijkheid over is voor een vakje, dan moet je deze wel invullen. Deze regel is ontzettend handig! Want zodra je dat vakje invult, kan het zomaar zijn dat er maar één optie overblijft voor een ander vakje. Die kun je vervolgens ook weer invullen. Typisch bij het oplossen van een Sudoku zul je merken dat je tegen het einde d.m.v. deze regel een heel hoop vakjes kan invullen.

Jij gaat straks een oplosser maken op basis van de "single possibility rule". Door het continue toepassen van de regel kan totdat de sudoku is opgelost. In psuedocode:

    while sudoku is not solved
        for every tile in sudoku
            if tile is empty and there is just one possibility
                fill in tile

Niet alle sudoku's zijn op deze manier op te lossen, zeker niet die sudoku's die als moeilijk worden bestempeld. Maar, de simpelere puzzels kunnen we d.m.v. dit algoritme wel oplossen.

## Sudoku's inladen
Implementeer allereerst de `load()` functie in `solver.py`. Deze functie accepteert een `filename` als argument en geeft de ingeladen sudoku terug. Een sudoku representeer je simpelweg met een lijst van lijsten, een 2D lijst.

Je kan jouw implementatie van `load()` in de interpreter testen:

    >>> from solver import load
    >>> load("easy/puzzle1.sudoku")
    [[7, 9, 0, 0, 0, 0, 3, 0, 1], [0, 0, 0, 0, 0, 6, 9, 0, 0], [8, 0, 0, 0, 3, 0, 0, 7, 6], [0, 0, 0, 0, 0, 5, 0, 0, 2], [0, 0, 5, 4, 1, 8, 7, 0, 0], [4, 0, 0, 7, 0, 0, 0, 0, 0], [6, 1, 0, 0, 9, 0, 0, 0, 8], [0, 0, 2, 3, 0, 0, 0, 0, 0], [0, 0, 9, 0, 0, 0, 0, 5, 4]]
    >>>

## Sudoku's printen
Het is handig om makkelijk inzicht te kunnen krijgen in wat je aan het programmeren bent. Daarom is het nu handig om de `show()` functie te implementeren. De `show()` functie print een sudoku uit als volgt:


    >>> from solver import load, show
    >>> show(load("easy/puzzle1.sudoku"))
    7 9 _   _ _ _   3 _ 1
    _ _ _   _ _ 6   9 _ _
    8 _ _   _ 3 _   _ 7 6

    _ _ _   _ _ 5   _ _ 2
    _ _ 5   4 1 8   7 _ _
    4 _ _   7 _ _   _ _ _

    6 1 _   _ 9 _   _ _ 8
    _ _ 2   3 _ _   _ _ _
    _ _ 9   _ _ _   _ 5 4
    >>>

Let op de details:

  - Eén spatie tussen elk vakje
  - Drie spaties tussen de blokken (horizontaal)
  - Eén witregel tussen de blokken (verticaal)
  - Voor de lege plekken print je een `_`

## Candidates
Om de Single Possibility Rule toe te passen moet je allereerst kunnen bepalen wat voor kandidaten er zijn in een leeg vakje. In een vakje kunnen de cijfers 1 t/m 9 worden ingevuld, behalve als één van die cijfers al is gebruikt in de kolom/rij/blok. Ofwel de kandidaten op een vakje x,y zijn:

    candidates(x,y) = 1..9 - numbers_row - numbers_col - numbers_block

Implementeer de functie `candidates()` in `solver.py`.

    >>> from sudoku import candidates, load
    >>> candidates(load("easy/puzzle1.sudoku"), 1, 1)
    {2, 3, 4, 5}
    >>>

> Tip: maak gebruik van sets!

## Oplossen d.m.v. Single Possibility Rule
Nu je een sudoku kan inladen, en kan bepalen welke kandidaten er zijn op elk vakje van het bord, kun je een oplosser maken die d.m.v. de Single Possibility Rule de sudoku oplost. Hieronder nogmaals de pseudocode:

    while sudoku is not solved
        for every tile in sudoku
            if tile is empty and there is just one possibility
                fill in tile

Implementeer nu `solve_rule` in `solver.py`.

> Als `solve_rule()` de gegeven sudoku niet kan oplossen, vul dan zoveel mogelijk in en `return` het resultaat. Voorkom een infinite loop ;)

# DFS solvers
Op basis van de simpele regelgebaseerde oplosser kunnen we niet alle sudoku puzzels oplossen. Je zou ervoor kunnen kiezen om meer regels/strategieën te implementeren, en zo meer puzzels aan te kunnen. Er zijn echter bijzonder veel verschillende soorten sudoku's en strategieën.

Het goede nieuws is, we hebben een computer tot onze beschikking die ontzettend snel kan rekenen. We zouden dus ook i.p.v. van te voren bedenken welk getal waar moet, ongeïnformeerd kunnen gaan zoeken naar een oplossing. Ofwel, gewoon getallen invullen, en kijken of het leidt naar een oplossing. Er zijn verschillende manieren om zo te zoeken. Je zou een sudoku kunnen pakken, en naar willekeur getallen invullen tot je of op een oplossing komt, of niet. Dan begin je gewoon opnieuw, totdat je uiteindelijk een oplossing tegenkomt. Deze manier van zoeken is puur willekeurig, maar als we heel snel kunnen "gokken" en controleren of iets een oplossing is, dan werkt het wel.

We kunnen ook gestructureerder aan de slag gaan. In plaats van willekeurig getallen in te vullen, kunnen we ook kijken welke getallen we in elk vakje kunnen invullen, en vervolgens al deze getallen één voor één invullen. Dan zijn er twee klassieke ongeïnformeerde zoekalgoritmes vanuit de literatuur: Breadth-First Search en Depth-First Search. Beide zijn zoekalgoritmes die op een bepaalde volgorde zoeken naar een oplossing.

Voor dit probleem focussen we ons op Depth-First Search. De naam verklapt het al een beetje, Depth-First Search duikt eerst de diepte in. Hoe moet je dit voor je zien? Stel je voor, je hebt een Sudoku puzzel en op plek `(x, y) = (1, 1)` heb je drie kandidaten `(1, 4, 8)`. Depth-First Search kiest voor de eerste kandidaat `1`, en gaat vervolgens verder op het volgende lege vakje, zeg `(x, y) = (1, 2)`. Daar wordt vervolgens ook voor de eerste kandidaat gekozen, enzovoort. Uiteindelijk kunnen er twee dingen gebeuren: Of de sudoku puzzel is opgelost, of je komt een leeg vakje tegen waar je geen kandidaten voor hebt. Het kan namelijk zo maar zijn dat een eerdere keuze, zoals zet `1` op `(x, y) = (1, 1)` er toe leidt dat je later vast komt te zitten. Zodra er geen kandidaten meer zijn voor een leeg vakje doet het Depth-First Search algoritme een stapje terug, en herziet de laatst gemaakte keuze. Is er geen andere keuze die gemaakt kan worden? Dan nog een stapje terug. Uiteindelijk ziet de zoekvolgorde er zo uit:

![](depth_first.png)

In het plaatje hierboven is elk rondje een sudoku bord, elke verbinding een keuze voor het invullen van het eerstvolgende lege vakje, en elke laag representeert het aantal gemaakte zetten. De getallen representeren de volgorde van het zoeken. We gaan van een sudoku bord (`1`) naar sudoku bord (`2`) naar (`3`) en uiteindelijk (`4`). Bij (`4`) zitten we vast met een leeg vakje zonder kandidaten, dus we doen een stap terug naar (`3`) en vervolgens naar (`5`).

## Let's talk code
Hoe programmeer je nu een DFS algoritme? DFS is een bekend algoritme, en ook goed gedocumenteerd in psuedocode. Dus niet code die je direct kan uitvoeren, maar een beschrijving zo dat je er snel code van kan maken. Zie hier psuedocode voor een iteratieve implementatie van DFS:

    1  function DFS-iterative(V):
    2      let S be a stack
    3      S.push(V)
    4      while S is not empty
    5          V = S.pop()
    6          for all candidates C from V do
    7              let W be a copy of V
    8              apply C to W
    9              if W is a solution do
    10                 return W
    11             S.push(W)

Dit is een iteratieve implementatie, dat houdt in dat deze implementatie geen gebruik maakt van recursie.

In de pseudocode zie je de term `stack` en verschillende operaties op een stack, namelijk `push` en `pop`. Een `stack` is een zogenaamde Abstract Datatype. Twee woorden die even wat uitleg nodig hebben. Een Datatype is bijvoorbeeld een lijst, array, dictionary, integer, float, etc. Iets dat data kan bevatten. Een Abstract Datatype is een Datatype die niet bestaat in de taal, maar die bovenop bestaande taalelementen gebouwd kan worden. Zo bestaan er bijvoorbeeld geen lijsten in de programmeertaal C, maar je zou een lijst wel kunnen implementeren bovenop C's arrays. Een lijst is in termen van de programmeertaal C dus een Abstract Datatype, maar in termen van Python een Concrete Datatype.

Een `stack` is dus een Abstract Datatype, maar wat is het? Een `stack` in het Nederlands is simpelweg een stapel. Het klassieke voorbeeld is een stapel borden. Je zet een nieuw bord bovenop de stapel, en als je een bord nodig hebt, pak je deze van boven de stapel weg. Dat is alles wat met een stack bedoelt wordt. Je kan elementen (borden) bovenop toevoegen (d.m.v. `push`), en van boven weer weghalen (d.m.v. `pop`). Maar een `stack` is dus een Abstract Datatype, deze bestaat niet in Python. Gelukkig kan je deze wel heel makkelijk implementeren, of minder formeel gezegd: we kunnen een lijst gebruiken om een stack voor te stellen. Kijk maar:

    Let S be a stack   =>   S = []
    S.push(V)          =>   S.append(V)
    V = S.pop()        =>   V = S.pop()

De methode `.pop()` bestaat dus al voor een lijst in Python, en we kunnen simpelweg `.append()` gebruiken voor `.push()`.

Nu is het aan jou om op basis van de bovenstaande pseudocode, de functie `solve_dfs_it` in `solver.py` te implementeren.

Je hoeft niet de variabele namen in de pseudocode aan te houden, het is beter als je deze vervangt door duidelijkere, descriptievere namen. Voor regel 8 in de pseudocode moet je een kopie aanmaken, hiervoor kun je `deepcopy()` gebruiken, als volgt:

    from copy import deepcopy
    V = []
    W = deepcopy(V)
    W.append(1)
    print(V)
    print(W)

De functie `deepcopy()` maakt een kopie van wat je meegeeft als argument. Een diepe kopie, oftewel als je een twee dimensionale lijst meegeeft om te kopieren, dan worden alle lijsten daarin ook gekopieert!

## Solve_dfs_rec

## (Opt) solve_dfs_gen

# Sudoku's genereren