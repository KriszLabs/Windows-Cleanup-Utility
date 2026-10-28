# Windows Cleanup Utility

Egyszerű, egyetlen fájlból álló Windows batch script, amely a rendszer ideiglenes mappáit (%TEMP%, C:\Windows\Temp, opcionálisan Prefetch) tisztítja, beépített biztonsági ellenőrzésekkel és naplózással. Nem telepít semmit, nem módosít rendszerbeállítást — csak a megadott temp-mappák tartalmát törli.

![Platform](https://img.shields.io/badge/platform-Windows%2010%2F11-blue)
![Shell](https://img.shields.io/badge/shell-cmd%20batch-lightgrey)
![License](https://img.shields.io/badge/license-MIT-green)

## Tartalom

- [Funkciók](#funkciók)
- [Biztonsági funkciók](#biztonsági-funkciók)
- [Követelmények](#követelmények)
- [Használat](#használat)
- [Naplózás](#naplózás)
- [Működés](#működés)
- [Ismert korlátok](#ismert-korlátok)
- [Hozzájárulás](#hozzájárulás)
- [Licenc](#licenc)

## Funkciók

| Csoport | Adatok |
|---|---|
| Jogosultság | Adminisztrátori jogosultság ellenőrzése, automatikus UAC emelés |
| Tisztítás | Felhasználói TEMP mappa tisztítása, C:\Windows\Temp tisztítása |
| Opcionális | Prefetch mappa tisztítás (bekapcsolható a scriptben) |
| Naplózás | Minden törlési művelet és hiba rögzítése |

## Biztonsági funkciók

| Védelem | Leírás |
|---|---|
| C:\ védelem | A gyökérkönyvtár soha nem kerül törlésre |
| C:\Windows védelem | A rendszerkönyvtár kizárva a törlési listából |
| C:\Users védelem | A felhasználói profil gyökér nem törölhető |
| Program Files védelem | Programkönyvtárak kizárva |
| Junction / Reparse Point ellenőrzés | Szimbolikus linkeket és junctionöket nem követ, hogy ne egy másik könyvtár tartalma törlődjön |
| Hibás törlések naplózása | A nem törölhető fájlok a naplóba kerülnek |

## Követelmények

- Windows 10 / 11
- Rendszergazdai jogosultság (a `C:\Windows\Temp` tisztításához) — a script automatikusan UAC-emelést kér
- Windows PowerShell (a jogosultság-ellenőrzéshez)

## Használat

1. Töltsd le a `windows-cleanup-utility.bat` fájlt (vagy klónozd a repót).
2. Futtasd dupla kattintással — a script automatikusan rendszergazdai jogot kér. Parancssorból:

   ```bat
   windows-cleanup-utility.bat
   ```

3. A futás végén egy billentyű lenyomásával bezárható az ablak.

> **Megjegyzés:** a Prefetch tisztítás alapból ki van kapcsolva; a script elején egy kapcsolóval engedélyezhető.

## Naplózás

A futás eredménye (törölt fájlok, kihagyott elemek, hibák) a következő fájlba kerül:

   ```text
   cleanup.log
   ```

## Működés

1. **Jogosultság-ellenőrzés.** A script ellenőrzi, hogy rendszergazdai jogokkal fut-e; ha nem, automatikusan UAC-emelést kér.
2. **Védett útvonalak listája.** A kritikus könyvtárak (`C:\`, `C:\Windows`, `C:\Users`, Program Files) kizárási listában szerepelnek.
3. **Reparse Point / Junction szűrés.** Törlés előtt minden elemet ellenőriz, hogy reparse point-e; így a junctionök és szimbolikus linkek nem követődnek be, nem törli más könyvtár tartalmát.
4. **Tisztítás.** A `%TEMP%` és `C:\Windows\Temp` mappák tartalmát törli (a védett elemek kihagyásával), opcionálisan a Prefetch mappát is.
5. **Naplózás.** Minden sikeres törlést, kihagyott elemet és hibát a `cleanup.log` fájlba ír.

## Ismert korlátok

- A Windows által zárolt fájlokat (pl. futó alkalmazások temp-fájljait) nem tudja törölni — ezek a naplóba kerülnek.
- A Prefetch tisztítás rövid távon lassíthatja az alkalmazások első indítását.
- Nem aláírt batch szkript: a Windows SmartScreen vagy az Intelligens alkalmazáskezelés figyelmeztetést jeleníthet meg futtatáskor. A forráskód teljes egészében megtekinthető és ellenőrizhető a repóban.
- Windows 10 vagy régebbi környezetben nem tesztelt.

## Hozzájárulás

Hibajelentést és javaslatot Issue-ban várok. Pull request esetén:

- Forkold a repót és hozz létre külön ágat (`feature/uj-funkcio`).
- Tartsd a `.bat` fájlt UTF-8 (BOM nélküli), CRLF sorvéggel.
- Új biztonsági vagy tisztítási funkció esetén frissítsd a README funkciótáblázatát és a `CHANGELOG.md` fájlt.
- A PR leírásában add meg, melyik Windows-verzión próbáltad ki.

## Licenc

[MIT](LICENSE)

## Állapot

Fejlesztés alatt — jelenleg **V2.0**  
Utolsó frissítés: 2026. 10. 27.
