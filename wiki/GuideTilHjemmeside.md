# Guide til hjemmeside
Denne guide beskriver, hvordan wiki-sider oprettes og redigeres.

**Forudsætninger:**

1.	[GitHub konto](https://github.com/) oprettet med RegionH mail med et identificerbart brugernavn
2.	Adgang til organisationsprofilen i GitHub [DataOgDigitalisering](https://github.com/DataOgDigitalisering) (gives af Admin: Stefan)

## Oprettelse af ny wiki-side
En ny side oprettes i Github Pages-repositoriet, [**_dokumentation_**](https://github.com/DataOgDigitalisering/dokumentation). Navigér ind I mappen *wiki*.  Klik på *’Add file’* -> *’Create new file’*. Navngiv din nye side *NavnPåSide.md* med fil-suffixet *.md* for at skabe en ny tom markdown fil.
Herfra kan indholdet til den nye wiki-side skrives ind med [**_markdown-syntax_**](https://www.markdownguide.org/basic-syntax/). Formateringen på indholdet kan løbende tjekkes ved at benytte *Preview* funktionen.

<details>
   <summary><b>Se gennemgang</b></summary>
   <center>
      <video autoplay controls muted loop="loop" width="718" height="611">
         <source src="https://github.com/DataOgDigitalisering/dokumentation/raw/master/Images/Nyfil.webm" type="video/webm">
      </video>
   </center>
</details>
<br>


Når indholdet er færdigt og filen er gemt, navigeres der ud til [roden](https://github.com/DataOgDigitalisering/dokumentation) og derefter ind til [*_includes*](https://github.com/DataOgDigitalisering/dokumentation/tree/master/_includes). Her åbnes filen [sidebar-html](https://github.com/DataOgDigitalisering/dokumentation/blob/master/_includes/sidebar.html). Denne skal man redigere ved at trykke på blyanten og tilføje følgende linje **uden** kommentarmarkeringen udenom ```<!-- -->```.

```html
<!-- <li><a href="{{'/SideNavn'| relative_url}}">Sidebar titel til side</a></li> -->.
```
SideNavn er det fil-navn, siden er gemt som under dokumentation/wiki/. Sidebar titel er det navn, der kommer til at stå i sidebaren, der linker til wiki-siden. Vent 3-5 minutter på at Github bygger siden og kontroller om det ser korrekt ud. Det er også muligt at referere til eksterne links, og hvis det ønskes at linket åbnes i en ny fane tilføjes ```target="_blank"``` i linjen. F.eks. 
```html
<li><a href="{{ 'https://www.markdownguide.org/cheat-sheet/'}}" target="_blank" >Markdown cheatsheet</a></li>
```
> **TIP:** Man kan følge med i byggeprocessen af siden på [Actionsfanen](https://github.com/DataOgDigitalisering/dokumentation/actions)

<details>
   <summary><b>Se gennemgang</b></summary>
   <center>
      <video autoplay controls muted loop="loop" width="718" height="611">
         <source src="https://github.com/DataOgDigitalisering/dokumentation/raw/master/Images/NyfilSidebar.webm" type="video/webm">
      </video>
   </center>
</details>

## Rediger eksisterende wiki-side
Åbn Github Page-repo’et, [**_dokumentation_**](https://github.com/DataOgDigitalisering/dokumentation) og navigér ind I mappen *wiki*. Åbn den side du ønsker at redigere. Klik på blyanten for at redigere. Når ændringer er foretaget, klikkes der på *Commit changes*.
Vent 5-10 minutter på at Github bygger siden. Kontroller om det ser korrekt ud. Browsere gemmer ofte en cache af siden, som kan få siden til at se ud, som om den ikke er opdateret, selvom den er. Her anbefales det, enten rydde ens cache, eller teste siden i en privat/inkognito-instans af din browser.

<details>
   <summary><b>Se gennemgang</b></summary>
   <center>
      <video autoplay controls muted loop="loop" width="718" height="611">
         <source src="https://github.com/DataOgDigitalisering/dokumentation/raw/master/Images/ændring.webm" type="video/webm">
      </video>
   </center>
</details>

## Privat side
Hvis en side ikke ønskes offentlig tilgængelig lægges den op i dette sekundære, men private [repository](https://github.com/DataOgDigitalisering/FortroligInformation). Der kan herefter linkes til siden fra sidebaren ved tilføje nedensåtende linje her [sidebar-html](https://github.com/DataOgDigitalisering/dokumentation/blob/master/_includes/sidebar.html).

```html
<li><a href="{{ 'DirekteLink'}}" target="_blank" >Sidebar titel til side</a></li>
```


## HTML
Det er desuden også muligt indlejre [HTML-kode](https://www.w3schools.com/html/default.asp) i markdown-filen. Der kan f.eks., som vist ovenover, inkluderes fold-ud sektioner og videoklips - videorne vil dog kun ses i den færdig byggede side og ikke gennem GitHubs *Preview* funktion. Dette blev gjort med følgende kode: 
```html
<details>
   <summary>Se gennemgang</summary>
   <video autoplay controls muted loop="loop" width="718" height="611">
      <source src="https://github.com/DataOgDigitalisering/dokumentation/raw/master/Images/ændring.webm" type="video/webm">
   </video>
</details>
```
Til videoer er det vigtigt at *autoplay* er sat som muted - ellers kan Chromium-baserede browsere ikke afspille dem.

Yderligere HTML funktionalitet, der kan implementeres, kan bl.a. findes [her](https://www.w3schools.com/html/default.asp).

## Konfigurering af siden
Store dele af wiki-siden konfigureres i [**_config.yml_**](https://github.com/DataOgDigitalisering/dokumentation/blob/master/_config.yml) filen. Her kan layoutet fuldstændig ændres ved at ændre [to linjer](http://www.drassil.org/git-wiki/customize) til nogle prædefinerede temaer. Det er også muligt at skrive sin egen CSS-fil med layout-konfigurationer, og erstatte de eksisterende.

### Scripting
Funktionalitet kan tilføjes til siden gennem scripting. F.eks. hentes der en matematik-syntaks fortolker (MathJax), så siden kan vise matematisk notation. 
Scripting kan lade sig gøre ved at navigere ind til [_includes/footer.html](https://github.com/DataOgDigitalisering/dokumentation/blob/master/_includes/footer.html)  referere til dit script i denne. **footer.html** bliver refereret i config_yml filen i Include Hooks-sektionen.

Et eksempel kunne være:
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.21.0/plugins/line-numbers/prism-line-numbers.min.js"></script>
```
Ovenstående script er et plugin, der tilføjer linjenummerering til kodeblokke.

### Brug af kodeblokke
En kodeblok kan genereres i markdown ved at benytte sig af **tre backticks** før og efter en kodeblok, f.eks. *\`\`\`* [Selve koden] *\`\`\`*. Såfremt man ønsker syntaks markeringer, gøres dette ved inkludere navnet på syntaksen efter de første tre backticks, dvs. 

 *\`\`\`dax* 
 
 [Selve koden] 
 
 *\`\`\`*

Syntaksmarkeringsredskabet, der benyttes er [Prism js](https://prismjs.com/#supported-languages), og det er valgt, da det understøtter DAX samt de mest gængse programmeringssprog. 

### Brug af matematisk notation
For at vise matematisk notation kan syntaksen kendt fra Tex og LaTex benyttes. For at gøre dette benyttes `$$` som afgrænserer rundt om notationen, f.eks. bliver  ```$$\sqrt{3x-1}+(1+x)^2$$``` til $$\sqrt{3x-1}+(1+x)^2$$
En reference til syntaksen for den matematiske notation kan findes [her](https://tilburgsciencehub.com/building-blocks/collaborate-and-share-your-work/write-your-paper/amsmath-latex-cheatsheet/).

### Teknisk opsætning af siden
Denne statiske side er genereret vha. [Jekyll](https://jekyllrb.com/) og hostet vha. [GitHub Pages](https://pages.github.com/). Skabelonen til siden er genereret [herfra](https://github.com/Drassil/git-wiki-skeleton).
