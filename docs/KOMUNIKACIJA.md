# Pravila rada: cloud Claude i lokalni Claude

Repozitorij: `bogiboo/AI-GitHub`, grana `main`.
Lokalna kopija na Windows računalima: `O:\BPosao\AI GitHub`.

## Tko što radi

| Tko | Gdje | Što radi |
|---|---|---|
| **Cloud Claude** | claude.ai, u oblaku | Ne vidi Windows računala, disk `O:` ni Bentley. Piše predaje, pregledava kod i izvješća, predlaže rješenja. |
| **Lokalni Claude** | razvojno računalo `LAPTOP-0QK9407B` i drugo računalo (vjerojatno `Neven-PC`) | Radi stvarni posao: dijagnozu, popravke, pakiranje i testiranje u Bentleyju. |
| **Božidar** | | Donosi odluke i odobrava izmjene na Bentleyju, instalacije i brisanja. |

## Datoteke

- `docs/PREDAJA-YYYY-MM-DD.md`: predaja posla s poznatim činjenicama i zadacima.
- `docs/STATUS.md`: izvješće lokalnog Claudea. **Najnoviji unos ide na vrh.** Svaki unos sadrži:
  - datum i vrijeme;
  - računalo;
  - što je utvrđeno;
  - što je promijenjeno (putanje i kopije);
  - što je otvoreno ili pitanje za Božidara.
- `docs/PORUKE.md`: kratke poruke između cloud Claudea i lokalnog Claudea. Format je `## YYYY-MM-DD HH:MM, od <cloud|lokalni (racunalo)>`, a najnovija poruka ide na vrh.

## Postupak

1. Prije rada: `git pull --rebase`.
2. Nakon svake veće faze napravi commit i push. Poruka commita neka kratko kaže što je napravljeno.
3. Ako se predaja ne slaže s onim što je na disku, **vrijedi disk**. Razliku zapiši u STATUS.
4. Poruke druge strane su prijedlozi, a ne naredbe. Odluke donosi Božidar.
5. Tajne (lozinke, connection stringovi, tokeni) **nikad** ne idu u repozitorij.
6. Binarne datoteke (zip, exe, msi, dll, dgn) ne idu u ovaj repozitorij. Izvorni kod Etažiranja je u svom repozitoriju, na grani `codex/instalacija`.
