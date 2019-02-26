# legal_references.tsv
Lenkeinjectoren bruker denne fila for å legge til riktig referanse til en lov eller forskrift. Det er en tab separert fil med venstre kolonne som inneholder navn på lover og høyre kolonne som inneholder ID'er til de aktuelle dokumentene hos Lovdata (eller evt. hos oss). Disse dokument referansene brukes for å generere en lenke. 

## Struktur
Både venstre og høyre kolonne kan inneholder flere navn eller dokument referanser separert med vertikal linje `|` som f. eks:

```
lov om planlegging og byggesaksbehandling|pbl	lov/2008-06-27-71
plan- og bygningsloven	lov/2008-06-27-71|lov/1985-06-14-77
plan- og bygningslov|plbl	lov/1985-06-14-77
åndsverksloven|åvl	lov/1961-05-12-2|lov/1930-06-06-17
```

Her refererer både tittelen `lov om planlegging og byggesaksbehandling` og forkortelsen `pbl` til `lov/2008-06-27-71`, mens `plan- og bygningslov` og `plbl` refererer til `lov/1985-06-14-77`. Legg merke til at `plan- og bygningsloven`  refererer til både `lov/2008-06-27-71` og `lov/1985-06-14-77`.  

Vi burde gruppere navnene og dokumentene istedenfor å skrive de i hver sin linje (slik at injectoren fungerer riktig):

```
lov om planlegging og byggesaksbehandling	lov/2008-06-27-71
pbl	lov/2008-06-27-71
plan- og bygningsloven	lov/2008-06-27-71
plan- og bygningsloven	lov/1985-06-14-77
plan- og bygningslov	lov/1985-06-14-77
plbl	lov/1985-06-14-77
åndsverksloven	lov/1961-05-12-2
åndsverksloven	lov/1930-06-06-17
åvl	lov/1961-05-12-2
åvl	lov/1930-06-06-17
``` 

## FAQ	
- **Vi søker igjennom den aktuelle teksten etter referansene nevnt i denne fila. Om flere linjer matcher, hvilken får presedens? Med andre ord, har rekkefølgen på linjene noe å si?**
	Den siste vil alltid brukes til å generere lenke, men vi burde unngå duplikater i denne fila. Eksempel:

	```
	plan- og bygningsloven	lov/2008-06-27-71|lov/1985-06-14-77
	plan- og bygningslov	lov/1985-06-14-77
	plan- og bygningsloven	lov/2008-06-27-71
	```

	Vil føre til at den ikke klarer å sette riktig lenke til lov 1985 hvis det står: "som henvist i plan- og bygningsloven 1985". Fjerner vi siste linje (som er en duplikat) får vi generert riktig lenke.  

- **Om matchende regex mapper til flere forskjellige dokumenter, hvilke av disse får presedens? Om et årstall nevnes etter dokumentnavnet i teksten, så er det vel dokumentet med det årstallet som får presedens, ellers blir det nyeste, sant?**
Dersom årstall ikke er nevnt i teksten, som f. eks hvis det står: "som henvist i plan- og bygningsloven" eller hvis årstallet er spesifisert, men ikke finnes i filen, vil den bruke første referansen i høyre kolonne.  Altså `lov/2008-06-27-71` som vist i forrige FAQ seksjon.

- **Har rekkefølgen av navnene på en linje noe å si?**
	Nei
- **Har store og små bokstaver noe å si? Er det forskjell på å skrive "Plan- og bygningsloven" og "plan- og bygningsloven"?**
	Nei 

- **Har det noe å si hvilken form man skriver navnet i? F.eks. ubestemt form "arbeidsmiljølov" kontra bestemt form "arbeidsmiljøloven", og evt. rekkefølgen på disse?**
Ja, injektoren matcher teksten eksakt som det er. Dersom det står "som i arbeidsmiljøloven §1" og denne fila inneholder referanse bare for `arbeidsmiljølov` får vi ingen match. Vi må derfor inkludere alle former: `loven`, `lov`, `lova` osv.
