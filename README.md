# Static content
Dette repositoriet benyttes hovedsakelig til lagring og publisering av nyheter, men også til lagring av annen informasjon vi mangler APIer for å hente ut.

> Benytt `development` branchen for å endre og legge til data. Endringer i denne branchen kan testes i stage.

## Nyheter
### Nyhetstyper
Vi har enn så lenge følgende nyhetstyper:
- **literature**: Benyttes for å opplyse om ny litteratur på Juridika
- **juridika**: Benyttes for å opplyse om ny funksjonalitet, planlagt nedetid osv.
- **news_article**: Innsikt artikkel
- **expert_comment**: Ekspert kommentar
- **serial**: Overordnet informasjon om en føljetong feed. Typisk informasjon som skal brukes i en header lik den for Jussens venner serien på innsikt.
- **serial_chapter**: Et kapittel i en føljetong.

### Begreper
- **Notis**: En notis er en kort nyhetstekst som vises i nyhetsfeedene på forsiden og på innsiktsiden.
- **Artikkel**: Selve artikkelteksten og metadata for nyhetene som har en helsides artikkel knyttet til seg. Typisk vil dette være en innsiktartikkel eller ekspertkommentar.

### Hvordan opprette en artikkel
Opprett en ny mappe i mappen `news_articles`. Bruk artikkeltittelen med bindestrek for å navngi den nye mappen. Opprett så en `json` og en `html` fil i mappen. Begge disse filene må ha samme navn som mappen de ligger i.

Du skal nå ha følgende mappestruktur:

    .
    ├── ...
    ├── news_articles                 # Innholder alle helsides nyhetsartikler
    │   ├── my_new_article            # Mappe for nyhetsartikkel
    │       ├── my_new_article.json   # Artikkel metadata
    │       ├── my_new_article.html   # Artikkel html
    │   └── ...
    └── ...

#### Metadata
Kopier json strukturen fra en annen artikkel og oppdater innholdet.

#### HTML
Ta utgangspunkt i HTMLen fra en annen artikkel eller i `news_articles/news_article_template.html` for å få riktig struktur på HTMLen.

Jeg pleier å benytte https://wordhtml.com/ for å konvertere Word til HTML. Kopier brødteksten fra wordfilen inn i verktøyet. Oppdater formateringen på alle overskrifter fra fet tekst til `Heading 3` (husk å fjerne formateringen for fet tekst) før du konverterer til HTML.

Gå til fanen `HTML`. Trykk på knappen med `£` symbolet for å skru av encoding. Trykk deretter på knappen `Clean` og til slutt på knappen for kode indentering. Nå kan du kopiere teksten å lime den inn i HTML dokumentet.

Bilder lastes opp til S3 bucketen `juridika-news-images` på Amazon.

### Hvordan opprette en notis
Alle notiser finnes i filen `news.json`

#### Notis uten tilhørende artikkel
Finn den nyeste notisen som har `id` satt til en tallverdi. Copy/paste og oppdater med korrekt info. Husk å oppdatere `id` ved å legge til 1.

#### Notis med tilhørende artikkel
Kopier innholdet fra json filen med metadata som ble opprettet når du opprettet artikkelen. Lim innholdet inn i `news.json`.

### Publiser
Etter å ha testet at alt ser riktig ut på https://stage.juridika.no kan du merge `development` branchen inn i `master`. Det er ikke nødvendig å opprette pull request. Etter merge vil endringene være tilgjengelige på [Juridika](https://juridika.no).
