# Status (najnovije na vrhu)

## 2026-11-04 12:40, lokalni (LAPTOP-0QK9407B) — installer: popravci s Neven-PC-a u izvornom kodu, novi paket

**Promijenjeno** (`bogiboo/Etaziranje`, grana `codex/instalacija`, commit `140a574`)
- Mapa paketa kad je `$PSScriptRoot` u `param()` prazan (mrežni disk): popravak s Neven-PC-a, doslovno.
- .NET provjera: `Get-DotnetRuntimes` — puna putanja `dotnet.exe`, stderr ne ruši provjeru; rezerva su mape
  `%ProgramFiles%\dotnet\shared\...` s `.deps.json`. Test je pritom našao stvarnu grešku (StrictMode pad
  kad `dotnet.exe` nije u PATH-u), popravljena.
- `Find-MapUltimate`: `HKLM\SOFTWARE\Bentley\MapUltimate\*\ApplicationPath`, Uninstall `InstallLocation`,
  prečaci u izborniku Start, zadana putanja. `-MapUltimateExe` i dalje ima prednost. Nađena putanja se ispisuje.
- Testovi installera: 14/14 (novi IN12 bez -Paket, IN13 .NET rezerva, IN14 MapUltimate).

**Novi paket**: `O:\BPosao\AI Installations\Etaziranje-v1.0-inst2.zip` (6795715 B, SHA-256 `f3a7fbb7…4f44b`).
Program i Bentley dio bajt-jednaki `Etaziranje-v1.0.zip`; razlikuju se samo `scripts/Instaliraj.ps1` i `PROCITAJ.txt`.

**Otvoreno**
- Proba na Neven-PC: suhi prolaz pa prava instalacija (pod Windows korisnikom koji će koristiti E).
- Ako MapUltimate na Neven-PC ni sada nije nađen: zapisati što je u `HKLM\SOFTWARE\Bentley\MapUltimate`.
- Odjeljci 3–4 predaje (cijeli WorkSpace, UCF higijena, inventura): opseg nakon probe na Neven-PC.

## 2026-11-04 12:00, lokalni (LAPTOP-0QK9407B) — odjeljak 1 predaje: zatvoren bez popravka

**Utvrđeno**
- Bentley se na razvojnom računalu **pokreće i radi** s Arhitekturom 2.0.2 i WorkSpaceom ETAZIRANJE_RAZVOJ.
  Proba 4.11. 11:50: prečac „ETAZIRANJE RAZVOJ” → početna stranica (WorkSpace ETAZIRANJE_RAZVOJ / TEST_OSNOVA)
  → otvoren `Z28-KUCA.dgn`; Bentley odgovara; `ribbon.log`: `registered 2.0.2.1` i
  `added tab ArhitekturaBlokovi.Etaziranje`. Božidar potvrdio da sve radi.
- Prividno „zapinjanje”: Bentley je čekao spajanje CONNECTION Clienta, a zatim stajao na početnoj stranici
  (čeka odabir datoteke). Dodaci iz `MS_DGNAPPS` učitavaju se tek pri otvaranju DGN-a, pa prije toga
  `ribbon.log` ne može imati zapis.
- Windows zapisi od 2.11.: samo jedan `AppHangTransient` 2.11. 18:18 (dok je Codex radio). Nema rušenja.

**Razlike predaja ↔ disk (vrijedi disk)**
- `Personal.ucf` **sadrži** blok ARHITEKTURA BLOKOVI MANAGED (bajt-jednak `Personal.ucf.s-arhitekturom`);
  vraćanje kopije `before-architecture-…` nije napravljeno. Blok ima `%if exists`.
- `ribbon.log` nije prazan: 2.11. 18:25–19:07 sedam uspješnih učitavanja, 4.11. 11:50 još jedno.
- `ArchitectureRibbon.dll` (7680 B) je od 2.11. **18:29** (ne 15:11); uz njega su dijagnostički
  `ArchitectureRibbonInspect.dll` i `ArchitectureRibbonViewInspect.dll` (2.11. 18:56 / 19:01).
- `ETAZIRANJE_RAZVOJ.cfg` (2.11. 12:57) je **izvorni** (SHA-256 `3ce2b78d…`): vrijeme je od završetka
  Z20 pilota na v1.0, koji vraća izvornu datoteku. Nije uzrok ničega.
- Bentley je 3.11. u 9:59 radio i uredno se zatvorio (tada zapisani `Personal.upf`, docking, RibbonState).

**Promijenjeno**: ništa (samo dva probna pokretanja Bentleyja).

**Otvoreno / sljedeće** (odobrio Božidar)
- Etažiranje na Neven-PC: popravak `$PSScriptRoot` (mrežni disk) i otpornija provjera .NET-a unose se u
  izvorni kod (`bogiboo/Etaziranje`, grana `codex/instalacija`), zatim traženje Bentleyja preko registra.
- Opseg odjeljaka 3 i 4 predaje (cijeli WorkSpace, UCF higijena, inventura) odlučuje se kad Etažiranje
  na Neven-PC proradi.
