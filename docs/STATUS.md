# Status (najnovije na vrhu)

## 2026-11-04 14:10, cloud (prema ispisima s Neven-PC) — ETAZ ON PANEL: Unknown key-in, uklonjen stari blok EON

**Utvrđeno**
- U Bentleyju (prečac „ETAZIRANJE RAZVOJ”, otvoren DGN) `ETAZ ON PANEL` javlja „Unknown key-in”, a
  `mdl load Etaziranje.Bentley` javlja „ETAZIRANJE_ON is already loaded”.
- Zaključak (vjerojatan, nije potvrđen popisom modula): stari blok `EON INIT MANAGED` u `Personal.ucf`
  (`EON_INIT_ROOT = C:/AITools/EtažiranjeBentleyBlokovi/`, `%include .../integration/eon/eon.cfg`, `MS_DGNAPPS > EonInit.dll`)
  učitava staru komponentu pod istim MdlTaskID `ETAZIRANJE_ON` (vidi `Etaziranje.Bentley/AddInMain.cs`). Novi
  `Etaziranje.Bentley.dll` iz `C:\AITools\EtaziranjeON\runtime\current` zato se ne učita, pa njegova KeyinTree ne postoji.

**Promijenjeno** (uz Božidarovo odobrenje)
- Iz `%LOCALAPPDATA%\Bentley\MapUltimate\25.0.0\prefs\Personal.ucf` uklonjen je **samo** blok
  `# BEGIN/END EON INIT MANAGED`. Kopija prije promjene: `Personal.ucf.prije-uklanjanja-EON-<datum-vrijeme>`, u istoj mapi.
- Blok `ThreeDtoBen T01 autoload` je ostao.

**Otvoreno**
- Proba: „ETAZIRANJE RAZVOJ” → DGN → `ETAZ ON PANEL`.
- **Za installer (razvojno računalo):** inventura i UCF higijena (odjeljak 3, točka 4 predaje) moraju otkriti blok
  `EON INIT MANAGED`, odnosno bilo koju drugu komponentu s MdlTaskID `ETAZIRANJE_ON`, i uz kopiju je ukloniti.
  Bez toga je instalacija na računalu sa starim EON-om „uspješna”, a ON ne radi. Završna provjera mora uključivati
  stvarno učitavanje novog `Etaziranje.Bentley.dll`.

## 2026-11-04 13:45, cloud (prema ispisima s Neven-PC) — Etažiranje v1.0-inst2 instaliran

**Utvrđeno**
- Računalo `Neven-PC`, Windows korisnik `geo\bozidar`, PowerShell bez administratorskih prava.
- Paket `Etaziranje-v1.0-inst2.zip` kopiran je u `C:\Install\Etaziranje-v1.0-inst2\`. SHA-256 odgovara.
- Suhi prolaz: 111/111 datoteka. **MapUltimate je pronađen automatski** na
  `C:\Program Files\Bentley\OpenCities Map Ultimate 2025\MapUltimate\MapUltimate.exe`.
  Preduvjeti (.NET 10, WebView2, .NET Framework 4.8) su u redu.
- Prava instalacija je završena. Provjera instaliranog: E 59/59, ON runtime 3/3, ON connector 10/10, ON config 1/1.
  **Baza: prijava E i SELECT 1 prošli.** Prečaci „Etaziranje E” i „ETAZIRANJE RAZVOJ” postoje.
- Zapis instalacije: `C:\AITools\INSTALACIJA-20261104-123037.txt`. U zapisu nema grešaka.
  Retci koje je filtar izdvojio su obavijesti (nema prethodne instalacije, uputa za `mdl load`).
- User varijable postoje (provjeren je samo `bool`): `ETAZIRANJE_E_APP_DATABASE_URL`, `ETAZIRANJE_ON_APP_DATABASE_URL`,
  `ETAZIRANJE_E_EXPECTED_ROLE`, `ETAZIRANJE_ON_EXPECTED_ROLE`.

**Promijenjeno**: instalacija prema paketu (`C:\AITools\EtaziranjeE`, `C:\AITools\EtaziranjeON`,
`C:\ProgramData\Bentley\GEODEZIJA_RAZVOJ\Configuration`, prečaci, User varijable). `Personal.ucf` nije diran
i i dalje ima blokove EON i ThreeDtoBen (15 redaka).

**Otvoreno**
- Proba prečaca „Etaziranje E” i „ETAZIRANJE RAZVOJ” (Bentley, otvaranje DGN-a, ON).
- `Personal.ucf` na Neven-PC: stari blokovi EON (`C:/AITools/EtažiranjeBentleyBlokovi`, bezuvjetni `%include`)
  i ThreeDtoBen T01 mogu se sudariti s novim ON-om. Ovo je odjeljak 3–4 predaje i odlučuje se nakon probe.
- Instalacija je pod korisnikom `geo\bozidar`. Ako E na Neven-PC koristi drugi korisnik, instalaciju treba ponoviti pod njim.

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
