# Static content

Dette repositoriet benyttes hovedsakelig til lagring av informasjon vi mangler APIer for å hente ut.

## Lov referanser

`link-injector-service` bruker `legal_references.tsv` for å lage riktig URL til lover i tekstene.
Første kolonne i TSV fila er en liste med titler/kort-titler mens andre kolonne er en liste av unike id'er som refererer til en lov.

Eksempel,

```
arbeidstidsloven	lov/1949-06-10-2|lov/1939-03-10-5
```

Vil bruke `lov/1949-06-10-2` for "arbeidstidsloven 1949"
