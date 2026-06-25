# Auftrag 3: IT-Konzept und Systemdokumentation

## Sota GmbH

**Modul:** 117  
**Thema:** ICT-Konzept für die Sota GmbH  
**Erstellt von:** Abedin Balanca  
**Standort:** Zürich  

---

## 1. Ausgangslage

Die Sota GmbH befindet sich in einem Gebäude auf einer Etage in Zürich. Die IT-Infrastruktur wird dezentral aufgebaut und lokal administriert.

Folgende Vorgaben gelten:

- Es gibt keinen zentralen Verzeichnisdienst.
- Es gibt keinen zentralen Datei-, DNS- oder DHCP-Server.
- Sämtliche PCs werden einzeln eingerichtet und administriert.
- Die Internetanbindung erfolgt über einen Internet Service Provider.
- DNS, Webhosting und E-Mail-Hosting werden durch den ISP bereitgestellt.
- Alle Netzwerkgeräte, Computer und Drucker erhalten eindeutige Namen.
- Die internen Geräte erhalten feste IPv4-Adressen.
- Gäste erhalten Adressen aus einem eigenen dynamischen Bereich.

---

## 2. Namenskonzept

### 2.1 Allgemeine Namensregeln

| Namensart | Namensregel | Beispiel |
|---|---|---|
| Anwendername | Erste drei Buchstaben des Nachnamens + erste drei Buchstaben des Vornamens | `DEREVA` für Eva Derungs |
| Arbeitsgruppe | `SOTA` | `SOTA` |
| Computer | `PC-xy` | `PC-01` |
| Router | `RT-xy` | `RT-01` |
| Switch | `SW-xy` | `SW-01` |
| Drucker | `PR-xy` | `PR-01` |

Alle Namen werden in Grossbuchstaben geschrieben.

### 2.2 Benutzerkonten

| Person | Funktion / Abteilung | Benutzername |
|---|---|---|
| Eva Derungs | Geschäftsleitung | `DEREVA` |
| Urs Kehl | Verkauf, Abteilungsleiter | `KEHURS` |
| Anton Meier | Verkauf, Region Zürich | `MEIANT` |
| Lorenz Fischer | Verkauf, Ostschweiz | `FISLOR` |
| Severin Lardo | Finanzen, Buchhaltung | `LARSEV` |
| Gian Binder | Finanzen, Budget und Planung | `BINGIA` |
| Luca Dali | Abwicklung | `DALLUC` |
| Theo Kranz | IT-Administration | `KRATHE` |

### 2.3 Lokale Arbeitsgruppe

Alle Computer werden der folgenden lokalen Windows-Arbeitsgruppe zugeordnet:

```text
SOTA
```

---

## 3. Gruppen- und Berechtigungskonzept

Da kein zentraler Verzeichnisdienst vorhanden ist, werden die Benutzerkonten und lokalen Gruppen auf den benötigten Computern einzeln eingerichtet.

### 3.1 Gruppen und Mitglieder

| Gruppe | Mitglieder |
|---|---|
| Verkauf | `KEHURS`, `MEIANT`, `FISLOR` |
| Finanzen | `LARSEV`, `BINGIA` |
| Abwicklung | `DALLUC` |
| GL | `DEREVA` |
| IT-Administration | `KRATHE` |

### 3.2 Berechtigungsmatrix

**Schreiben** beinhaltet ebenfalls das Lesen und Ändern von Dateien.

| Gruppe | Mitglieder | Verkauf | Finanzen | Abwicklung |
|---|---|---|---|---|
| Verkauf | `KEHURS`, `MEIANT`, `FISLOR` | Schreiben | Lesen | Lesen |
| Finanzen | `LARSEV`, `BINGIA` | Lesen | Schreiben | Lesen |
| Abwicklung | `DALLUC` | Lesen | Lesen | Schreiben |
| GL | `DEREVA` | Schreiben | Schreiben | Schreiben |
| IT-Administration | `KRATHE` | Schreiben | Schreiben | Schreiben |

### 3.3 Verzeichnisberechtigungen

| Verzeichnis | Leserecht | Schreibrecht | Aufgabe |
|---|---|---|---|
| `C:\Sota\` | Alle Mitarbeitenden | Alle Mitarbeitenden | Mutterverzeichnis für alle Gruppenordner |
| `C:\Sota\Verkauf` | Verkauf, Finanzen, Abwicklung, GL, IT-Administration | Verkauf, GL, IT-Administration | Ablage für Angebote, Kundendaten und Verkaufsunterlagen |
| `C:\Sota\Finanzen` | Verkauf, Finanzen, Abwicklung, GL, IT-Administration | Finanzen, GL, IT-Administration | Ablage für Buchhaltung, Budget und Finanzunterlagen |
| `C:\Sota\Abwicklung` | Verkauf, Finanzen, Abwicklung, GL, IT-Administration | Abwicklung, GL, IT-Administration | Ablage für Auftragsabwicklung, Termine und Lieferdokumente |

### 3.4 Umsetzung der Berechtigungen

Die Ordner werden lokal erstellt:

```text
C:\Sota
C:\Sota\Verkauf
C:\Sota\Finanzen
C:\Sota\Abwicklung
```

Die Zugriffsrechte werden über lokale Windows-Gruppen vergeben. Direkte Einzelberechtigungen sollen möglichst vermieden werden. Theo Kranz erhält als IT-Administrator Vollzugriff, damit er die Umgebung warten und Fehler beheben kann.

---

## 4. Adresskonzept

### 4.1 IPv4-Netzwerk

| Eigenschaft | Wert |
|---|---|
| Netzwerkadresse | `192.168.100.0` |
| Subnetzmaske | `255.255.255.0` |
| CIDR-Präfix | `/24` |
| Nutzbarer Hostbereich | `192.168.100.1` bis `192.168.100.254` |
| Broadcast-Adresse | `192.168.100.255` |
| Standardgateway | `192.168.100.1` |
| Externe IP-Adresse | `62.23.12.245` |
| Primärer DNS-Server | `62.23.12.245` |
| Sekundärer DNS-Server | `62.23.12.246` |

### 4.2 Adressbereiche

| Geräteart | Zuteilung | Adressbereich |
|---|---|---|
| Router und Switches | Feste, manuell konfigurierte IP-Adressen | `192.168.100.1` bis `192.168.100.20` |
| Drucker | Feste, manuell konfigurierte IP-Adressen | `192.168.100.31` bis `192.168.100.50` |
| Computer | Feste, manuell konfigurierte IP-Adressen | `192.168.100.51` bis `192.168.100.150` |
| Gäste | Dynamische Zuteilung | `192.168.100.151` bis `192.168.100.200` |
| Reserve | Für zukünftige Geräte | `192.168.100.201` bis `192.168.100.254` |

---

## 5. Netzwerkkomponenten und IP-Adresszuteilung

### 5.1 Router und Switches

| Gerät | Typ | IP-Adresse | Standort |
|---|---|---|---|
| `RT-01` | ADSL-Router / Firewall | intern: `192.168.100.1`<br>extern: `62.23.12.245` | Hauptverteiler beim Arbeitsplatz des IT-Administrators im Verkauf |
| `SW-01` | Fast-Gigabit-Switch / Hauptswitch | `192.168.100.2` | Hauptverteiler beim Arbeitsplatz des IT-Administrators im Verkauf |
| `SW-02` | Standard-Switch | `192.168.100.3` | Bereich GL / Finanzen |
| `SW-03` | Standard-Switch | `192.168.100.4` | Bereich Abwicklung |

### 5.2 Computer

| Gerät | Benutzer | Abteilung | IP-Adresse | Standort |
|---|---|---|---|---|
| `PC-01` | Eva Derungs | Geschäftsleitung | `192.168.100.51` | GL / Finanzen |
| `PC-02` | Severin Lardo | Finanzen | `192.168.100.52` | Büro Finanzen |
| `PC-03` | Gian Binder | Finanzen | `192.168.100.53` | Büro Finanzen |
| `PC-04` | Luca Dali | Abwicklung | `192.168.100.54` | Abwicklung |
| `PC-05` | Theo Kranz | IT-Administration | `192.168.100.55` | Verkauf, beim Hauptverteiler |
| `PC-06` | Lorenz Fischer | Verkauf | `192.168.100.56` | Verkauf |
| `PC-07` | Anton Meier | Verkauf | `192.168.100.57` | Verkauf |
| `PC-08` | Urs Kehl | Verkauf | `192.168.100.58` | Verkauf / Sitzungszimmer |

### 5.3 Netzwerkdrucker

| Gerät | Typ | IP-Adresse | Standort |
|---|---|---|---|
| `PR-01` | Netzwerk-Laserdrucker | `192.168.100.31` | Verkauf |
| `PR-02` | Netzwerk-Laserdrucker | `192.168.100.32` | Abwicklung |
| `PR-03` | Netzwerk-Laserdrucker | `192.168.100.33` | GL / Finanzen |

---

## 6. Netzwerktopologie

Die Sota GmbH verwendet eine erweiterte Sterntopologie.

```text
ADSL-Internet
      |
    RT-01
      |
    SW-01
   /  |  \
  /   |   \
SW-02 |  SW-03
  |   |     |
GL /  |  Abwicklung
Fin.  |
      Verkauf / IT
```

### 6.1 Physische Verbindungen

| Von | Nach | Zweck |
|---|---|---|
| ADSL-Anschluss | `RT-01` | Internetanbindung |
| `RT-01` | `SW-01` | Verbindung des internen Netzwerks mit Router und Firewall |
| `SW-01` | `SW-02` | Uplink zum Bereich GL / Finanzen |
| `SW-01` | `SW-03` | Uplink zum Bereich Abwicklung |
| `SW-01` | `PC-05` bis `PC-08` und `PR-01` | Verkauf und IT-Administration |
| `SW-02` | `PC-01` bis `PC-03` und `PR-03` | Geschäftsleitung und Finanzen |
| `SW-03` | `PC-04` und `PR-02` | Abwicklung |

### 6.2 Verkabelung

Für die feste Gebäudeverkabelung wird mindestens Cat-6a-Kupferverkabelung eingesetzt. Die Verbindungen werden über den Hauptverteiler, ein Patchpanel und die Switches geführt.

Gemäss dem Grundriss sind für die bekannten Strecken ungefähr 81 Meter Kabel eingezeichnet. Wegen Leitungsführung, Reserven, Verschnitt und Rangierung wird eine zusätzliche Reserve eingeplant.

Empfohlene Bestellmenge:

```text
Mindestens 100 Meter Cat-6a-Installationskabel
```

---

## 7. Gerätekonfiguration

### 7.1 Einheitliche IPv4-Konfiguration

Jedes interne Gerät erhält:

```text
Subnetzmaske:       255.255.255.0
Standardgateway:    192.168.100.1
Primärer DNS:       62.23.12.245
Sekundärer DNS:     62.23.12.246
```

Die jeweilige Geräte-IP wird gemäss der Tabellen in Kapitel 5 eingetragen.

### 7.2 Router `RT-01`

Der Router übernimmt folgende Aufgaben:

- Verbindung zum Internet Service Provider
- Network Address Translation
- Firewall-Funktion
- Standardgateway für alle internen Geräte
- Dynamische Adressvergabe für Gäste im Bereich `192.168.100.151` bis `192.168.100.200`
- Weiterleitung von DNS-Anfragen an die DNS-Server des Providers

### 7.3 Switches

Die Switches erhalten feste Management-Adressen:

```text
SW-01: 192.168.100.2
SW-02: 192.168.100.3
SW-03: 192.168.100.4
```

Nicht verwendete Switchports werden deaktiviert, sofern die Geräte dies unterstützen. Die Switches werden beschriftet und die verwendeten Ports in der Systemdokumentation festgehalten.

### 7.4 Computer

Auf jedem Computer werden folgende Schritte durchgeführt:

1. Computername gemäss Namenskonzept setzen.
2. Arbeitsgruppe `SOTA` eintragen.
3. Feste IPv4-Adresse konfigurieren.
4. Lokales Benutzerkonto erstellen.
5. Benutzer den benötigten lokalen Gruppen zuordnen.
6. Betriebssystem aktualisieren.
7. Virenschutz und lokale Firewall aktivieren.
8. Netzwerkdrucker der jeweiligen Abteilung installieren.
9. Zugriff auf die vorgesehenen Verzeichnisse testen.
10. Konfiguration dokumentieren.

### 7.5 Drucker

Die Drucker werden mit einer festen IP-Adresse eingerichtet und unter ihrem eindeutigen Namen installiert:

```text
PR-01: 192.168.100.31
PR-02: 192.168.100.32
PR-03: 192.168.100.33
```

Auf den Computern werden die passenden Treiber installiert. Anschliessend wird pro Drucker eine Testseite gedruckt.

---

## 8. Lokale Administration

Theo Kranz ist für die lokale Administration der ICT-Umgebung verantwortlich.

Seine Aufgaben sind:

- Installation und Konfiguration der Computer
- Verwaltung der lokalen Benutzerkonten und Gruppen
- Vergabe und Prüfung der Berechtigungen
- Installation von Updates und Software
- Konfiguration von Netzwerkgeräten und Druckern
- Pflege der Systemdokumentation
- Durchführung von Datensicherungen
- Unterstützung der Mitarbeitenden bei Störungen

Für administrative Tätigkeiten wird ein separates Administratorkonto verwendet. Das normale Benutzerkonto soll nicht dauerhaft mit Administratorrechten arbeiten.

---

## 9. Sicherheitskonzept

Folgende Mindestmassnahmen werden umgesetzt:

- Persönliche Benutzerkonten für alle Mitarbeitenden
- Sichere und unterschiedliche Passwörter
- Keine gemeinsame Nutzung persönlicher Konten
- Separate Administratoranmeldung
- Aktivierte Windows-Firewall
- Aktueller Virenschutz
- Regelmässige Betriebssystem- und Softwareupdates
- Feste Zugriffsrechte auf die Abteilungsverzeichnisse
- Gesicherte Router- und Switch-Konfiguration
- Änderung aller Standardpasswörter
- Dokumentation der Geräte und IP-Adressen
- Regelmässige Datensicherung wichtiger Geschäftsdaten
- Sperrung des Computers bei Abwesenheit
- Deaktivierung nicht benötigter Dienste und Anschlüsse

---

## 10. Datensicherung

Da kein zentraler Dateiserver eingesetzt wird, müssen geschäftskritische Daten besonders sorgfältig gesichert werden.

Empfohlene Umsetzung:

- Tägliche automatische Sicherung der wichtigen lokalen Daten
- Verschlüsselte externe Sicherung
- Mindestens eine Sicherungskopie ausserhalb des jeweiligen Computers
- Regelmässige Wiederherstellungstests
- Dokumentation der Sicherungsintervalle und Aufbewahrungsfristen
- Zugriff auf Sicherungen nur für die Geschäftsleitung und IT-Administration

---

## 11. Test- und Abnahmeplan

Nach der Installation werden folgende Tests durchgeführt:

| Test | Erwartetes Ergebnis |
|---|---|
| Verkabelungstest | Alle Netzwerkleitungen sind korrekt aufgelegt und fehlerfrei |
| Ping auf `RT-01` | Router unter `192.168.100.1` erreichbar |
| Ping auf alle Switches | `SW-01`, `SW-02` und `SW-03` erreichbar |
| Internetzugriff | Alle berechtigten Computer erreichen das Internet |
| DNS-Test | Namen im Internet werden korrekt aufgelöst |
| IP-Prüfung | Jedes Gerät besitzt die dokumentierte, eindeutige Adresse |
| Computernamen | Alle PCs entsprechen dem Namenskonzept |
| Arbeitsgruppe | Alle PCs sind Mitglied der Arbeitsgruppe `SOTA` |
| Druckertest | Auf jedem Drucker kann eine Testseite gedruckt werden |
| Verkauf-Berechtigung | Verkauf kann im Ordner Verkauf schreiben und die anderen Ordner lesen |
| Finanzen-Berechtigung | Finanzen kann im Ordner Finanzen schreiben und die anderen Ordner lesen |
| Abwicklung-Berechtigung | Abwicklung kann im Ordner Abwicklung schreiben und die anderen Ordner lesen |
| GL-Berechtigung | Geschäftsleitung kann alle Abteilungsordner lesen und bearbeiten |
| IT-Berechtigung | IT-Administration besitzt den benötigten administrativen Zugriff |
| Neustarttest | Einstellungen bleiben nach einem Neustart erhalten |
| Dokumentationskontrolle | Gerätenamen, Adressen und Standorte stimmen mit der Dokumentation überein |

---

## 12. Vollständige Systemübersicht

| Name | Typ | Zugeordnet zu | IP-Adresse | Anschluss |
|---|---|---|---|---|
| `RT-01` | Router / Firewall | Gesamtes Netzwerk | `192.168.100.1` | Internet und `SW-01` |
| `SW-01` | Fast-Gigabit-Switch | Hauptverteiler | `192.168.100.2` | `RT-01`, `SW-02`, `SW-03`, Verkauf / IT |
| `SW-02` | Standard-Switch | GL / Finanzen | `192.168.100.3` | `SW-01`, `PC-01` bis `PC-03`, `PR-03` |
| `SW-03` | Standard-Switch | Abwicklung | `192.168.100.4` | `SW-01`, `PC-04`, `PR-02` |
| `PR-01` | Drucker | Verkauf | `192.168.100.31` | `SW-01` |
| `PR-02` | Drucker | Abwicklung | `192.168.100.32` | `SW-03` |
| `PR-03` | Drucker | GL / Finanzen | `192.168.100.33` | `SW-02` |
| `PC-01` | Computer | Eva Derungs | `192.168.100.51` | `SW-02` |
| `PC-02` | Computer | Severin Lardo | `192.168.100.52` | `SW-02` |
| `PC-03` | Computer | Gian Binder | `192.168.100.53` | `SW-02` |
| `PC-04` | Computer | Luca Dali | `192.168.100.54` | `SW-03` |
| `PC-05` | Computer | Theo Kranz | `192.168.100.55` | `SW-01` |
| `PC-06` | Computer | Lorenz Fischer | `192.168.100.56` | `SW-01` |
| `PC-07` | Computer | Anton Meier | `192.168.100.57` | `SW-01` |
| `PC-08` | Computer | Urs Kehl | `192.168.100.58` | `SW-01` |

---

## 13. Fazit

Das IT-Konzept erfüllt die Vorgaben der Sota GmbH und passt zum vorhandenen Grundriss sowie zur geplanten Beschaffung aus den vorherigen Teilaufträgen.

Die Lösung verwendet:

- acht lokal administrierte Computer,
- drei Netzwerkdrucker,
- einen Router mit Firewall,
- einen zentralen Fast-Gigabit-Switch,
- zwei zusätzliche Bereichs-Switches,
- ein einheitliches Namens- und Adresskonzept,
- klar geregelte Verzeichnisberechtigungen,
- eine strukturierte Cat-6a-Netzwerkverkabelung.

Durch die eindeutige Dokumentation können die Geräte später einfacher gewartet, ersetzt oder erweitert werden.
