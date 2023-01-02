# Guide til hjemmeside
Denne guide beskriver hvordan man opretter og redigere wiki-sider.
Forudsætninger

1.	[GitHub account](https://github.com/) oprettet med RegionH mail med et identificerbart brugernavn
2.	Adgang til organisationen-profilen i GitHub [DataOgDigitalisering](https://github.com/DataOgDigitalisering) (gives af Admin: Stefan)

## Oprettelse af ny wiki-side
En ny side oprettes i Github Pages-repositoriet, [**_dokumentation_**](https://github.com/DataOgDigitalisering/dokumentation). Navigér ind I mappen *wiki*.  Klik på *’Add file’* -> *’Create new file’*. Navngiv din nye side *NavnPåSide.md* med fil-suffixet *.md* for at skabe en ny tom markdown fil.
Herfra kan indholdet til den nye wiki-side skrives ind med [**_markdown-syntax_**](https://www.markdownguide.org/basic-syntax/). Formateringen på indholdet kan løbende tjekkes ved at benytte *Preview* funktionen.

<video autoplay="autoplay muted" loop="loop" width="718" height="611">
    <source src="https://github.com/DataOgDigitalisering/dokumentation/raw/master/Images/Nyfil.webm" type="video/webm">
</video>


Når indholdet er færdigt og filen er gemt, navigeres der ud til [roden](https://github.com/DataOgDigitalisering/dokumentation) og derefter ind til [*_includes*](https://github.com/DataOgDigitalisering/dokumentation/tree/master/_includes). Her åbnes filen [sidebar-html](https://github.com/DataOgDigitalisering/dokumentation/blob/master/_includes/sidebar.html). Denne skal man redigere ved at trykke på blyanten og tilføje følgende linje 

```html
<li><a href="{{'/SideNavn'| relative_url}}">Sidebar titel til side</a></li>.
```

SideNavn er det fil-navn, siden er gemt som under dokumentation/wiki/. Sidebar titel er det navn, der kommer til at stå i sidebaren, der linker til wiki-siden.
Vent 5-10 minutter på at Github bygger siden. Kontroller om det ser korrekt ud. 
> **TIP:** Man kan følge med i byggeprocessen af siden på [Actionsfanen](https://github.com/DataOgDigitalisering/dokumentation/actions)

## Rediger eksisterende wiki-side
Åbn Github Page-repo’et, [**_dokumentation_**](https://github.com/DataOgDigitalisering/dokumentation) og navigér ind I mappen *wiki*. Åbn den side du ønsker at redigere. Klik på blyanten for at redigere. Når ændringer er foretaget, klikkes der på *Commit changes*.
Vent 5-10 minutter på at Github bygger siden. Kontroller om det ser korrekt ud. Browsere gemmer ofte en cache af siden, dom kan få siden til at se ud, som om den ikke er opdateret, selvom den er. Her anbefales det, enten rydde ens cache, eller teste siden i en privat/inkognito-instans af din browser.

<video autoplay="autoplay muted" loop="loop" width="718" height="611">
    <source src="https://github.com/DataOgDigitalisering/dokumentation/raw/master/Images/ændring.webm" type="video/webm">
</video>


## Konfigurering af siden
Store dele af wiki-siden konfigureres i [**_config.yml_**](https://github.com/DataOgDigitalisering/dokumentation/blob/master/_config.yml) filen. Her kan layoutet fuldstændig ændres ved at ændre [to linjer](http://www.drassil.org/git-wiki/customize) til nogle prædefinerede temaer. Det er også muligt at skrive sin egen CSS-fil med layout-konfigurationer.

### Scripting
Funktionalitet kan tilføjes til siden gennem scripting. F.eks. hentes der en matematik-syntaks fortolker (MathJax), så siden kan vise matematisk notation. 
Scripting kan lade sig gøre ved at navigere ind til [_includes/footer.html](https://github.com/DataOgDigitalisering/dokumentation/blob/master/_includes/footer.html)  referere til dit script i denne. **footer.html** bliver refereret i config_yml filen i Include Hooks-sektionen.

Et eksempel kunne være:
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.21.0/plugins/line-numbers/prism-line-numbers.min.js"></script>
```
Ovenstående script tilføjer linjenummerering til kodeblokke.

### Brug af kodeblokke
En kodeblok kan genereres i markdown ved at benytte sig af **tre backticks** før og efter en kodeblok, f.eks. *\`\`\`* [Selve koden] *\`\`\`*. Såfremt man ønsker syntaks markeringer, gøres dette ved inkludere navnet på syntaksen efter de første tre backticks, dvs. 

 *\`\`\`dax* 
 
 [Selve koden] 
 
 *\`\`\`*

Syntaksmarkeringsredskabet, der benyttes er [Prism js](https://prismjs.com/#supported-languages), og det er valgt, da det understøtter DAX samt de mest gængse programmeringssprog. 

### Brug af matematisk notation
For at vise matematisk notation kan syntaksen kendt fra Tex og LaTex benyttes. Hvis man vil bruge matematisk notation **in-line**, dvs. midt i tekst benyttes `$$` afgrænserer rundt om notationen, f.eks.  $$\sqrt{3x-1}+(1+x)^2$$

### Teknisk opsætning af siden
Denne statiske side er genereret vha. [Jekyll](https://jekyllrb.com/) og hostet vha. [GitHub Pages](https://pages.github.com/). Skabelonen til siden er genereret [herfra](https://github.com/Drassil/git-wiki-skeleton).
