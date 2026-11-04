# Poruke

## 2026-11-04 12:50, od lokalni (LAPTOP-0QK9407B) → lokalni (Neven-PC)

Ti si Claude na drugom računalu (vjerojatno `Neven-PC`, potvrdi s `hostname`). Zadatak: **vodi Božidara
kroz instalaciju Etažiranja v1.0-inst2 na ovom računalu**, korak po korak, i javi ishod.

**Prije rada pročitaj:** `docs/KOMUNIKACIJA.md` (pravila), vrh `docs/STATUS.md` (što je napravljeno),
`docs/PREDAJA-2026-11-04.md` odjeljak 2 (stanje ovog računala). Kratko:
- Odjeljak 1 predaje je zatvoren: Bentley na razvojnom računalu radi.
- Popravci s ovog računala (`$PSScriptRoot` na mrežnom disku, .NET provjera) sada su u izvornom kodu
  (`bogiboo/Etaziranje`, `codex/instalacija`, `140a574`), plus traženje MapUltimate preko registra.
- Novi paket: `O:\BPosao\AI Installations\Etaziranje-v1.0-inst2.zip`, SHA-256
  `f3a7fbb7c7089152c7f488d065d324bdf0098a5b829a487b205b0f578fb4f44b`. Program je bajt-jednak v1.0.

**Pravila (obavezna):** hrvatski; tajne (konekcije, ključevi) nikad ne ispisuj, ne traži u chatu i ne
zapisuj — Božidar ih sam lijepi u skriveni upit installera; prije svega što mijenja Bentley, instalira
softver ili briše datoteke pitaj Božidara. `Personal.ucf` (EON / ThreeDtoBen blokovi) **sada ne diraj** —
to je kasniji korak (odjeljci 3–4). Stari paket `O:\...\Etaziranje-v1.0\` ostavi kakav jest.

**Koraci**
1. **Provjere (ništa ne mijenjaju):** `hostname`, `whoami` — instalacija mora ići pod Windows korisnikom
   koji će koristiti E (User varijable i prečaci idu u njegov profil); ako to nije trenutni korisnik, stani
   i pitaj Božidara. Provjeri radi li E ili Bentley (`Get-Process Etaziranje,MapUltimate`).
2. **Paket na lokalni disk:** kopiraj ZIP u novu praznu mapu, npr. `C:\Install\Etaziranje-v1.0-inst2\`,
   provjeri SHA-256 prema gornjem i raspakiraj (`Expand-Archive`). Ne pokreći ništa s `O:`.
3. **Suhi prolaz:** u raspakiranoj mapi `Etaziranje-v1.0-inst2` pokreni
   `powershell -NoProfile -ExecutionPolicy Bypass -File scripts\Instaliraj.ps1 -Suho`
   (isto što `INSTALIRAJ.cmd -Suho`). Očekivano: 111 datoteka odgovara, preduvjeti u redu, redak
   `MapUltimate: <putanja>`.
   - Ako MapUltimate nije nađen: ispiši `Get-ItemProperty 'HKLM:\SOFTWARE\Bentley\MapUltimate\*' |
     Select ApplicationPath,ProgramPath,Version` i Uninstall unose s „OpenCities Map Ultimate” (samo putanje),
     zapiši u STATUS i nastavi s `-MapUltimateExe "<putanja>"` ako je putanja pouzdana.
   - Ako nešto nedostaje (.NET, WebView2): javi Božidaru što i odakle; ne instaliraj sam.
4. **Prava instalacija — tek kad Božidar kaže „može”:** neka on dvoklikne `INSTALIRAJ.cmd` (upit za
   konekciju je interaktivan i skriven; konekciju E lijepi on). Ti prati i tumači ispis.
5. **Provjera:** zapis `C:\AITools\INSTALACIJA-*.txt` (najnoviji) — sve stavke OK, „Baza: prijava E i SELECT 1
   prošli”; Božidar pokrene prečac „Etaziranje E” i potvrdi da se otvara.
6. **Izvješće:** novi unos na vrh `docs/STATUS.md` (format iz KOMUNIKACIJA.md: datum i vrijeme, računalo,
   utvrđeno, promijenjeno, otvoreno), pa `git pull --rebase`, commit, push. Bez tajni.

Ako installer padne: ne popravljaj paket na disku. Zapiši točnu poruku, redak i zapis u STATUS i javi —
popravak ide u izvorni kod na razvojnom računalu, pa novi paket.

## 2026-11-04, od cloud

Predaja je u `docs/PREDAJA-2026-11-04.md`, a pravila rada u `docs/KOMUNIKACIJA.md`. Lokalni Claude: pročitaj oboje, provjeri tvrdnje na disku i kreni od odjeljka 1 predaje. Izvješća piši u `docs/STATUS.md`.
