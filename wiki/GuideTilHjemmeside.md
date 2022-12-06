# Guide til hjemmeside
Denne guide beskriver hvordan man opretter og redigere wiki-sider.
Forudsætninger

1.	GitHub account oprettet med RegionH mail med et identificerbart brugernavn
2.	Adgang til organisationen-profilen i GitHub DataOgDigitalisering (gives af Admin: Stefan)

## Oprettelse af ny wiki-side
En ny side oprettes i Github Pages-repositoriet, [**_dokumentation_**](https://github.com/DataOgDigitalisering/dokumentation). Navigér ind I mappen ’wiki’.  Klik på ’Add file’ -> ’Create new file’. Navngiv din nye side ’NavnPåSide.md’ med fil-suffixet ’.md’ for at skabe en ny tom markdown fil.
Herfra kan indholdet til den nye wiki-side skrives ind med [**_markdown-syntax_**](https://www.markdownguide.org/basic-syntax/). Formateringen på indholdet kan løbende tjekkes ved at benytte ”Preview” funktionen.
Når indholdet er færdigt og filen er gemt, navigeres der ud til roden og derefter ind til ’_includes’. Her åbnes filen ’sidebar-html’. Denne skal man redigere ved at trykke på blyanten og tilføje følgende linje
```
<li><a href="{{ '/NavnPåSide| relative_url }}">Sidebar titelnavn til side</a></li>.
```

NavnPåSide er det fil-navn, siden er gemt som under dokumentation/wiki/. SidebarNavnTilSide er det navn, der kommer til at stå i sidebaren, der linker til wiki-siden.
Vent 5-10 minutter på at Github bygger siden. Kontroller om det ser korrekt ud.

## Rediger eksisterende wiki-side
Åbn Github Page-repo’et, [**_dokumentation_**](https://github.com/DataOgDigitalisering/dokumentation) og navigér ind I mappen ’wiki’. Åbn den side du ønsker at redigere. Klik på blyanten for at redigere. Når ændringer er foretaget, klikkes der på ’Commit changes’.
Vent 5-10 minutter på at Github bygger siden. Kontroller om det ser korrekt ud.

## Konfigurering af siden
Store dele af wiki-siden konfigureres i [**_config.yml_**](https://github.com/DataOgDigitalisering/dokumentation/blob/master/_config.yml) filen. Her kan layoutet fuldstændig ændres ved at ændre [to linjer](http://www.drassil.org/git-wiki/customize). Det er også muligt at skrive sin egen CSS-fil med layout-konfigurationer.
### Scripting
Funktionalitet kan tilføjes til siden gennem scripting. F.eks. er dag/nat-tilstand en funtion tilføjet gennem scripting. Desuden hentes der en matematik-syntaks fortolker (MathJax), så siden kan vise matematisk notation. 
Scripting kan lade sig gøre ved at navigere ind til _includes  og enten redigere en eksisterende HTML-fil eller tilføje en ny, og referere til dit script i denne.
Et eksempel kunne være:
```
<script src="/dokumentation/_includes/darkscript.js"></script>
```
Hvis det er en ny HTML-fil skal der efterfølges refereres til denne i config_yml filen i Include Hooks-sektionen.



