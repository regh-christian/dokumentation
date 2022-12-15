# Git & GitHub 
Med versionsstyring kan man let kan holde styr på ændringer i filer. Det vigtigt at differentiere mellem Git og GitHub.
## Git
Git er et open-source versionsstyringssystem, som kan hentes via Softwareshoppen. For at kontrollere at du har Git installeret, kan du køre følgende linje i din terminal/kommandoprompt ```git –version```. 

## GitHub
GitHub er en service (ejet af Microsoft), hvor Git-projekter kan hostes på en ekstern server (cloud). Vi bruger også GitHub til at hoste denne wiki.

## Arbejdsgang med Git & GitHub
Man kan vælge enten bruge Git med kommandoer gennem terminalen eller vha. et brugergrænseflade program som f.eks. GitHub Desktop. Denne kan også hentes via softwareshoppen.

### Arbejdsflowet med Git & GitHub

* Initialisér Git på projektmappen, så der skabes et lokalt repository - Eller ```git clone``` et eksisterende projekt
* Git opretter en skjult mappe, som registerer alle ændringer
* Når en fil, bliver ændret, tilføjet eller slettet registreres det som en ændring
* Herefter vælges de ændrede filer som skal ```git stage```.
* De staged filer skal ```git commit```, hvilket får Git til at gemme et permanemt "snapshot" af filerne
* Sidst laves et ```git push``` for lægge ændringer op i GitHub 

### Nyt Projekt
Når et nyt projekt startes, som skal versionsstyres, skal der oprettes en folder til projektet. Herefter skabes der et git repository ved at køre kommandoen git init i stien til den nyoprettede folder. Der vil nu blive oprettet en skjult mappe .git, som er git repository’et. Den sørger for registrere og gemme alle ændringer, der sker på filer i projektmappen – et såkaldt lokalt repository.

For at kunne samarbejde med andre og have backup af projekter er det dog vigtigt at benytte sig repositories, der ligger i ”skyen”. Det er her GitHub kommer ind i billedet, hvor man ”pusher” sit lokale repository til en webserver på GitHub.

### Eksisterende projekt
Hvis du skal arbejde på et eksisterende projekt, der ligger på github kan du ”clone” denne ned på din computer lokalt, arbejde på den, og derefter ”pushe” den op på ”skyen” igen, hvor ændringerne bliver flettet ind. 
Næste gang du arbejder projektet kan du så lave et ”pull” i stedet for ”clone”, for at hente ændringer som andre har lavet på projektet. 

### Mere læsning

[Basics om brug af git og GitHub](https://www.freecodecamp.org/news/learn-the-basics-of-git-in-under-10-minutes-da548267cc91/)

[Teknisk forklaring om git](https://how-to.dev/how-git-stores-data)

