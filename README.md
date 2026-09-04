# IT-Security

## Salt vs Pepper
Salt ist in der Datenbank unverschlüsselt gespeichert und wird nur genutzt, damit Angreifer die Hashes nicht mit vorberechneten Hashes vergleichen können.

Pepper wird genau wie der Salt mit in die Hashfunktion gegeben, wird allerdings im Gegensatz zum Salt nicht in der Datenbank gespeichert, sondern beispielsweise im geschützten TPM-Chip.

## Hashcat
Modes: (https://hashcat.net/wiki/doku.php?id=hashcat)[https://hashcat.net/wiki/doku.php?id=hashcat]

`hashcat -m 11600 [hash-file] [wordlist] -r [*.rule]`

Ohne -m detektiert hashcat den Hash, wenn bereinigt. Ohne -r probiert er einfach alle Words durch.

Mit --show wird nur ein bisheriges Ergebnis angezeigt, kein neuer Run gestartet.

### Hash detektieren
`hashcat [hash-file]` gibt den Hash-Algorithmus mit der Hashcat-Modus-Nummer heraus.

## 2john-Tools
Hierbei immer am Anfang den Dateinamen und den ersten Doppelpunkt entfernen, wenn der Hash mit hashcat verarbeitet werden soll. Manche Hashes (wie z. B. bei LibreOffice) müssen am Ende noch `:::::xyz` entfernt kriegen.

Wenn zwei Modi für einen Dateityp vorhanden sind, hilft es meistens, beide zu probieren. Wenn wie bei ODF unterschiedliche Hash-Algorithmen zu Grunde liegen, bricht Hashcat bei dem Falschen automatisch ab, da die Hash-Länge hier nicht übereinstimmt.
