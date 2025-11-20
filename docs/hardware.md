# Hardware-Anforderungen

## Übersicht

Diese Seite listet alle benötigten Hardware-Komponenten für den Aufbau der mobilen Feuerwehr-Infrastruktur auf.

## Minimalausstattung

### Raspberry Pi Setup

| Komponente | Spezifikation | Zweck | Geschätzte Kosten |
|------------|---------------|-------|-------------------|
| **Raspberry Pi 4 Model B** | 4 GB RAM (min. 2 GB) | Zentrale Recheneinheit | ca. 60-80 € |
| **microSD-Karte** | 32 GB (Class 10/UHS-1) | Betriebssystem & Daten | ca. 10-15 € |
| **Netzteil USB-C** | 5V / 3A (15W) | Stromversorgung | ca. 10-15 € |
| **Gehäuse** | Mit Lüftung | Schutz & Kühlung | ca. 10-20 € |

**Gesamtkosten Minimal:** ca. 90-130 €

## Empfohlene Ausstattung

### Erweiterte Konfiguration

| Komponente | Spezifikation | Zweck | Geschätzte Kosten |
|------------|---------------|-------|-------------------|
| **Raspberry Pi 4/5** | 8 GB RAM | Bessere Performance bei mehreren Streams | ca. 80-100 € |
| **microSD-Karte** | 64 GB (UHS-3/A2) | Mehr Speicher, schnellere I/O | ca. 15-20 € |
| **USB-C Powerbank** | 20.000 mAh, PD, 5V/3A | Mobile Stromversorgung | ca. 30-50 € |
| **Robustes Gehäuse** | IP65-Schutz, Aluminium | Outdoor-Einsatz | ca. 30-50 € |
| **Kühlkörper/Lüfter** | Aktive Kühlung | Dauerbetrieb unter Last | ca. 10-15 € |
| **HDMI-Kabel** | 2-3 Meter | Verbindung zu Monitor | ca. 5-10 € |

**Gesamtkosten Empfohlen:** ca. 170-245 €

## Optionale Komponenten

### Für erweiterte Funktionalität

| Komponente | Spezifikation | Zweck | Geschätzte Kosten |
|------------|---------------|-------|-------------------|
| **Tragbarer Monitor** | 10-15 Zoll, HDMI, USB-C Power | Mobile Anzeige | ca. 80-150 € |
| **Smartphone** | LTE-fähig, USB-Tethering | Internet-Zugang | bereits vorhanden |
| **USB-C zu USB-A Adapter** | Für Smartphone-Verbindung | Tethering | ca. 5-10 € |
| **Externe USB-SSD** | 128-256 GB | Video-Aufzeichnung | ca. 30-60 € |

## Detaillierte Spezifikationen

### Raspberry Pi

**Empfohlene Modelle:**
- **Raspberry Pi 4 Model B (4/8 GB)** - Bewährt, gute Unterstützung
- **Raspberry Pi 5 (8 GB)** - Neueste Generation, mehr Performance

**Mindestanforderungen:**
- RAM: mindestens 2 GB (4 GB empfohlen für Streaming)
- WLAN: 802.11ac (5 GHz) - integriert bei Pi 4/5
- Ethernet: Gigabit - für Uplink-Verbindungen
- USB: USB 3.0 Ports - für externe Speicher
- HDMI: Für Monitor-Ausgabe

### microSD-Karte

**Wichtige Eigenschaften:**
- **Kapazität:** Mindestens 32 GB (64 GB empfohlen)
- **Geschwindigkeit:** Class 10 oder UHS-1 (mindestens)
- **Zuverlässigkeit:** Markenprodukt (SanDisk, Samsung, etc.)
- **Application Performance Class:** A1 oder A2 für bessere I/O

### Stromversorgung

**Stationärer Betrieb:**
- Offizielles Raspberry Pi USB-C Netzteil (15W)
- 5V / 3A garantiert
- Kurze, dicke USB-C Kabel nutzen

**Mobiler Betrieb:**
- Powerbank mit USB-C PD (Power Delivery)
- Mindestens 5V / 3A Ausgang
- Kapazität: 20.000 mAh für ca. 6-8 Stunden Betrieb
- Pass-Through Charging unterstützt (aufladen während Nutzung)

**Empfohlene Powerbanks:**
- Anker PowerCore
- RAVPower PD Powerbanks
- UGREEN PD Powerbanks

### Gehäuse

**Anforderungen:**
- Gute Belüftung oder aktive Kühlung
- Zugang zu allen Ports
- Montageoptionen (VESA, DIN-Schiene)
- Outdoor: IP65-Rating für Staub/Wasserschutz

**Typen:**
- **Standard:** Kunststoff mit Lüftungsschlitzen (10-15 €)
- **Gekühlt:** Mit Lüfter oder Kühlkörper (15-25 €)
- **Robust:** Aluminium/IP-rated (30-60 €)
- **Tactical:** Militärstandard, schockresistent (80-150 €)

## Peripherie-Geräte

### Monitor/Display

**Mobile Monitore:**
- 10-15 Zoll Bildschirmdiagonale
- HDMI oder USB-C Eingang
- Eigene Stromversorgung oder USB-C Powered
- Ideal: Touch-Display für Interaktion

**Festinstallierte Monitore:**
- Beliebiger HDMI-Monitor/TV im Fahrzeug
- Empfohlen: 24-32 Zoll für Einsatzleitung

### Tablets & Smartphones

**Für Stream-Empfang:**
- Android 8.0+ oder iOS 12+
- WLAN 802.11ac (5 GHz) empfohlen
- WebRTC-fähiger Browser oder dedizierte App

**Für LTE-Tethering:**
- LTE-fähiges Smartphone
- USB-Tethering Support
- Ausreichendes Datenvolumen

## Montage & Transport

### Befestigungsoptionen

| Methode | Anwendung | Kosten |
|---------|-----------|--------|
| **VESA-Mount** | Montage an Monitor-Rückseite | 10-20 € |
| **DIN-Schienen-Halter** | Einbau in Schaltschrank | 15-30 € |
| **Klett-/Klebeband** | Temporäre Befestigung | 5-10 € |
| **Pelicase-Einbau** | Transportkoffer-Lösung | 50-150 € |

### Transport-Lösungen

- **Kleine Tasche:** Für Minimal-Setup (10-20 €)
- **Hartschalenkoffer:** Für komplettes System (30-80 €)
- **Pelicase:** Maximaler Schutz (100-200 €)
- **19" Rack-Einbau:** Für Fahrzeug-Integration (50-150 €)

## Einkaufslisten

### Basis-Set (Einsteiger)

```
□ Raspberry Pi 4B (4GB)          ~75 €
□ microSD 32GB                   ~12 €
□ Offizielles Netzteil           ~12 €
□ Gehäuse mit Lüfter             ~15 €
□ HDMI-Kabel                      ~7 €
                        Summe: ~121 €
```

### Profi-Set (Mobil)

```
□ Raspberry Pi 4B (8GB)          ~95 €
□ microSD 64GB (A2)              ~18 €
□ USB-C Powerbank 20Ah           ~45 €
□ Robustes Gehäuse IP65          ~40 €
□ 13" Portabler Monitor         ~120 €
□ HDMI-Kabel                      ~7 €
□ USB-C Adapter                   ~8 €
□ Transporttasche                ~25 €
                        Summe: ~358 €
```

### Vollausstattung (Fahrzeug)

```
□ Raspberry Pi 5 (8GB)          ~100 €
□ microSD 128GB (PRO)            ~30 €
□ POE HAT                        ~25 €
□ Rack-Gehäuse                   ~50 €
□ USB-SSD 256GB                  ~45 €
□ Netzwerk-Switch                ~20 €
□ DIN-Schienen-Mount             ~20 €
□ Festeinbau-Komponenten         ~50 €
                        Summe: ~340 €
```

## Bezugsquellen

**Deutschland:**
- [reichelt.de](https://www.reichelt.de) - Elektronik-Fachhändler
- [berrybase.de](https://www.berrybase.de) - Raspberry Pi Spezialist

## Wartung & Ersatzteile

**Empfohlene Ersatzteile:**
- Spare microSD-Karte mit vorinstalliertem Image
- USB-C Ersatzkabel
- Ersatz-Netzteil/Powerbank
- Backup Raspberry Pi für kritische Einsätze

**Lebensdauer:**
- Raspberry Pi: 5-10 Jahre
- microSD-Karte: 2-5 Jahre (je nach Nutzung)
- Powerbank: 300-500 Ladezyklen
- Lüfter: 3-5 Jahre

## Hinweise

⚠️ **Wichtig:**
- Verwenden Sie qualitativ hochwertige Stromversorgung
- Nutzen Sie kurze, dicke USB-C Kabel
- Bei Outdoor-Einsatz: IP-geschützte Komponenten verwenden
- Backup-Image der microSD-Karte erstellen
- Regelmäßige Wartung und Updates einplanen

💡 **Tipp:**
- Bei mehreren Fahrzeugen: Identische Hardware verwenden
- Ersatzteile zentral bevorraten
- Standardisierte Setups erleichtern Wartung
