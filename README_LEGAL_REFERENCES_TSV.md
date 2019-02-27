# legal_references.tsv
Lenkeinjectoren bruker denne fila for å oppdage referanser til lover og forskrifter fra deres navn. Det er en tab separert fil med venstre kolonne som inneholder navn på lover og høyre kolonne som inneholder ID'er til de aktuelle dokumentene hos Lovdata (eller evt. hos oss). Disse dokument referansene brukes for å generere lenker. 

## Struktur
De to kolonnene er separert med tab ('	') og kan inneholde flere navn eller dokument referanser separert med vertikal linje `|`:

```
lovtittel1|lovtittel2|lovtittel3	lov/xxxx-xx-xx-xx|lov/xxxx-xx-xx-xx
```

Det er viktig at hvert navn oppstår kun én gang (altså på kun én linje) i fila, for ellers blir det tvetydig hvilken linje som skal brukes til matchen. Altså, unngå eksempler som dette, der flere linjer inneholder samme navn:

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

Så først må alle linjer med samme navn og slås sammen til én linje der høyre kolonne inneholder alle dokument-IDene navnene kan referere til:

```
lov om planlegging og byggesaksbehandling	lov/2008-06-27-71
pbl	lov/2008-06-27-71
plan- og bygningsloven	lov/2008-06-27-71|lov/1985-06-14-77
plan- og bygningslov	lov/1985-06-14-77
plbl	lov/1985-06-14-77
åndsverksloven	lov/1961-05-12-2|lov/1930-06-06-17
åvl	lov/1961-05-12-2|lov/1930-06-06-17
```

Neste steg (ikke nødvendig, men er greit for lesbarhetens skyld og for å holde antall linjer i fila nede) er å slå sammen alle linjer som der høyre kolonne inneholder de samme dokument-IDene. Dette påvirker ikke resultatene injectoren gir (annet enn en eventuell ytelsesmessig forbedring):

```
lov om planlegging og byggesaksbehandling|pbl	lov/2008-06-27-71
plan- og bygningsloven	lov/2008-06-27-71|lov/1985-06-14-77
plan- og bygningslov|plbl	lov/1985-06-14-77
åndsverksloven|åvl	lov/1961-05-12-2|lov/1930-06-06-17
```

(Disse to stegene kan selvfølgelig gjøres i én, så lenge du holder tunga rett i munnen)

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
	Dersom navnet i teksten er etterfulgt av et årstall, så brukes dokumentet med dette årstallet. Hvis ikke brukes første referansen i høyre kolonne. Med eksempelet fra forrige FAQ seksjon, "som henvist i plan- og bygningsloven 1985." gir `lov/1985-06-14-77`, mens "som henvist i plan- og bygningsloven." gir `lov/2008-06-27-71`.

- **Har rekkefølgen av navnene på en linje noe å si?**
	Nei
- **Har store og små bokstaver noe å si? Er det forskjell på å skrive "Plan- og bygningsloven" og "plan- og bygningsloven"?**
	Nei 

- **Har det noe å si hvilken form man skriver navnet i? F.eks. ubestemt form "arbeidsmiljølov" kontra bestemt form "arbeidsmiljøloven", og evt. rekkefølgen på disse?**
	Ja, injektoren matcher teksten eksakt som det er. Dersom det står "som i arbeidsmiljøloven §1" og denne fila inneholder referanse bare for `arbeidsmiljølov` får vi ingen match. Vi må derfor inkludere alle former: `loven`, `lov`, `lova` osv.
