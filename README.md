# IT-Security

## Hashcat

`hashcat -m 11600 [hash-file] [wordlist] -r [*.rule]`

Ohne -m detektiert hashcat den Hash, wenn bereinigt. Ohne -r probiert er einfach alle Words durch.

Mit --show wird nur ein bisheriges Ergebnis angezeigt, kein neuer Run gestartet.
