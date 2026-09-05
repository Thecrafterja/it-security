# IT-Security

## Salt vs Pepper
Salt ist in der Datenbank unverschlüsselt gespeichert und wird nur genutzt, damit Angreifer die Hashes nicht mit vorberechneten Hashes vergleichen können.

Pepper wird genau wie der Salt mit in die Hashfunktion gegeben, wird allerdings im Gegensatz zum Salt nicht in der Datenbank gespeichert, sondern beispielsweise im geschützten TPM-Chip.

## Hashcat
Modes: [https://hashcat.net/wiki/doku.php?id=hashcat](https://hashcat.net/wiki/doku.php?id=hashcat)

`hashcat -m 11600 [hash-file] [wordlist] -r [*.rule]`

Ohne -m detektiert hashcat den Hash, wenn bereinigt. Ohne -r probiert er einfach alle Words durch.

Mit --show wird nur ein bisheriges Ergebnis angezeigt, kein neuer Run gestartet.

### Verschiedene Kombinationen
Angabe mit `-a [nummer]`:
Nummer|Beschreibung
--|--
0|Straight
1|Combination
3|Brute-force
6|Hybrid Wordlist + Mask
7|Hybrid Mask + Wordlist
9|Association

### Hash detektieren
`hashcat [hash-file]` gibt den Hash-Algorithmus mit der Hashcat-Modus-Nummer heraus.

### Masking in Hashcat

Beim Masking (Angriffsmodus `-a 3`) generiert Hashcat Passwörter nach einem exakt vorgegebenen Muster. Statt alle Kombinationen blind durchzuprobieren, definieren Sie für jede Position des Passworts die erlaubte Zeichengruppe. Das verkleinert den Suchraum im Vergleich zu reinem Brute-Force enorm.

### Standard-Platzhalter (Built-in Charsets)

| Platzhalter | Zeichenklasse | Enthaltene Zeichen |
|---|---|---|
| `?l` | Kleinbuchstaben | `a-z` |
| `?u` | Großbuchstaben | `A-Z` |
| `?d` | Ziffern | `0-9` |
| `?s` | Sonderzeichen | `` !"#$%&'()*+,-./:;<=>?@[\]^_`{|}~ `` |
| `?a` | Alle Zeichen | Kombination aus `?l`, `?u`, `?d`, `?s` |
| `?b` | Bytes | `0x00 - 0xff` (alle 256 Bytewerte) |

---

### Funktionsweise am Beispiel

Für ein vermutetes Passwort nach dem Schema `Sommer2024!` (Großbuchstabe, 5 Kleinbuchstaben, 4 Ziffern, Sonderzeichen):

* **Maske:** `?u?l?l?l?l?l?d?d?d?d?s`
* **Befehl:** `hashcat -a 3 -m [HASH_TYP] hash.txt ?u?l?l?l?l?l?d?d?d?d?s`

Feste Begriffe lassen sich direkt in die Maske schreiben. `Sommer?d?d?d?d` probiert beispielsweise nur das Wort "Sommer" kombiniert mit vier beliebigen Zahlen aus.

### Benutzerdefinierte Zeichensätze (Custom Charsets)

Über die Parameter `-1` bis `-4` können eigene Zeichengruppen definiert werden:

* **Szenario:** 4 Hexadezimalzeichen gefolgt von 2 Ziffern
* **Befehl:** `hashcat -a 3 -m 0 -1 0123456789abcdef hash.txt ?1?1?1?1?d?d`

## 2john-Tools
Hierbei immer am Anfang den Dateinamen und den ersten Doppelpunkt entfernen, wenn der Hash mit hashcat verarbeitet werden soll. Manche Hashes (wie z. B. bei LibreOffice) müssen am Ende noch `:::::xyz` entfernt kriegen.

Wenn zwei Modi für einen Dateityp vorhanden sind, hilft es meistens, beide zu probieren. Wenn wie bei ODF unterschiedliche Hash-Algorithmen zu Grunde liegen, bricht Hashcat bei dem Falschen automatisch ab, da die Hash-Länge hier nicht übereinstimmt.
