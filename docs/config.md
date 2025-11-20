# Konfiguration & Setup

Schritt-für-Schritt Anleitung zur Einrichtung des Raspberry Pi als mobiler WLAN Access Point mit Bridge-Funktionalität.

> **Basis:** Diese Anleitung basiert auf [Andreas Happe's Blog](https://snikt.net/blog/2019/06/22/building-an-lte-access-point-with-a-raspberry-pi/)

## 🎯 Zielsetzung

Wir konfigurieren den Raspberry Pi als:
- **WLAN Access Point** (5 GHz, 802.11ac)
- **Bridge** zwischen WLAN und Ethernet
- **DHCP/DNS Server** für automatische IP-Vergabe
- **Routing** für optionalen Internet-Zugang via Smartphone

## 📋 Voraussetzungen

- Raspberry Pi 4 oder 5 mit Raspberry Pi OS (Debian-basiert)
- microSD-Karte mit mindestens 16 GB
- SSH- oder direkter Zugriff (Monitor + Tastatur)
- Internet-Verbindung für die Installation

## 🚀 Schritt 1: System aktualisieren

Zunächst das System auf den neuesten Stand bringen:

```bash
sudo apt update
sudo apt upgrade -y
```

**Empfohlen:** Nach Updates neu starten:
```bash
sudo reboot
```

## 🌉 Schritt 2: Bridge konfigurieren

Die Bridge verbindet WLAN und Ethernet zu einem gemeinsamen Netzwerk.

**Bridge-Tools installieren:**

```bash
sudo apt install bridge-utils -y
```

**Netzwerk-Konfiguration** bearbeiten:

```bash
sudo nano /etc/network/interfaces
```

Folgenden Inhalt einfügen:

```bash
# Include files from /etc/network/interfaces.d:
source-directory /etc/network/interfaces.d

# Wired interface (Ethernet) - Manuell, wird Teil der Bridge
auto eth0
allow-hotplug eth0
iface eth0 inet manual

# Wireless interface (WLAN) - Manuell, wird Teil der Bridge
auto wlan0
allow-hotplug wlan0
iface wlan0 inet manual
wireless-power off  # Power-Saving deaktivieren für stabile Verbindung

# Bridge (br0) - Kombiniert eth0 und wlan0
auto br0
iface br0 inet static
        address 192.168.66.1        # Gateway-IP für das Netzwerk
        netmask 255.255.255.0       # Subnetzmaske (192.168.66.0/24)
        bridge_ports eth0 wlan0     # Verbundene Interfaces
        bridge_fd 0                 # Forward Delay auf 0 (schnellerer Start)
        bridge_stp off              # Spanning Tree Protocol aus (nicht benötigt)
```

**Speichern:** `Ctrl+O`, `Enter`, dann `Ctrl+X`

> **Hinweis:** Die IP `192.168.66.1` ist das Gateway für alle verbundenen Geräte.

## 🌐 Schritt 3: DHCP/DNS Server konfigurieren

Der DHCP-Server vergibt automatisch IP-Adressen an verbundene Geräte.

**dnsmasq installieren:**

```bash
sudo apt install dnsmasq -y
```

**Konfiguration erstellen:**

```bash
sudo nano /etc/dnsmasq.conf
```

Folgende Zeilen am Ende der Datei hinzufügen (oder bestehende überschreiben):

```bash
# Interface für DHCP/DNS
interface=br0

# DHCP-IP-Bereich (50-150 = 100 mögliche Clients)
dhcp-range=192.168.66.50,192.168.66.150,12h

# DHCP-Optionen
dhcp-option=3,192.168.66.1    # Gateway (dieser Raspberry Pi)
dhcp-option=6,192.168.66.1    # DNS-Server (dieser Raspberry Pi)

# DNS-Upstream-Server (falls Internet verfügbar)
server=8.8.8.8               # Google DNS
server=8.8.4.4               # Google DNS (Backup)

# Lokale Domain (optional)
domain=feuerwehr.local
local=/feuerwehr.local/

# Logging (optional, für Debugging)
log-queries
log-dhcp
```

**Speichern:** `Ctrl+O`, `Enter`, dann `Ctrl+X`

> **Tipp:** Der IP-Bereich kann bei Bedarf erweitert werden (z.B. .50-.200 für mehr Geräte)

## 📡 Schritt 4: WLAN Access Point konfigurieren

Der Access Point ermöglicht WLAN-Verbindungen zum Raspberry Pi.

**hostapd installieren und aktivieren:**

```bash
sudo apt install hostapd -y
sudo systemctl unmask hostapd
sudo systemctl enable hostapd
```

**Konfigurationsdatei erstellen:**

```bash
sudo nano /etc/hostapd/hostapd.conf
```

Folgenden Inhalt einfügen:

```bash
# ========================================
# Grundlegende Konfiguration
# ========================================

# Bridge-Interface (verbindet WLAN mit Ethernet)
bridge=br0

# WLAN-Interface
interface=wlan0
driver=nl80211

# Ländercode (DE für Deutschland, AT für Österreich, CH für Schweiz)
country_code=DE

# ========================================
# SSID und Sicherheit
# ========================================

# Netzwerkname (SSID) - WICHTIG: Anpassen!
# Empfehlung: Funkrufname des Fahrzeugs (z.B. "Florian-Stadt-11-1")
ssid=Florian-Fahrzeug-1

# WPA2-Verschlüsselung
wpa=2
wpa_key_mgmt=WPA-PSK
rsn_pairwise=CCMP

# WLAN-Passwort - WICHTIG: Ändern!
# Mindestens 8 Zeichen, besser 16+
wpa_passphrase=Feuerwehr2024!Sicher

# ========================================
# Access Control
# ========================================

# MAC-Adress-Filterung deaktiviert (0 = alle erlaubt)
macaddr_acl=0

# ========================================
# Logging (optional für Debugging)
# ========================================

logger_syslog=0
logger_syslog_level=4
logger_stdout=-1
logger_stdout_level=0

# ========================================
# WLAN-Modus: 5 GHz (802.11ac)
# ========================================

# Hardware-Modus: a = 5 GHz
hw_mode=a

# WMM (Wi-Fi Multimedia) für bessere QoS
wmm_enabled=1

# Kanal (36 = 5180 MHz, wenig Störungen)
# Alternativen: 40, 44, 48, 149, 153, 157, 161
channel=36

# ========================================
# 802.11n (WLAN-N) Konfiguration
# ========================================

ieee80211n=1
require_ht=1
ht_capab=[MAX-AMSDU-3839][HT40+][SHORT-GI-20][SHORT-GI-40][DSSS_CCK-40]

# ========================================
# 802.11ac (WLAN-AC) Konfiguration
# ========================================

ieee80211ac=1
require_vht=1

# DFS (Dynamic Frequency Selection) deaktiviert für schnelleren Start
ieee80211d=0
ieee80211h=0

# AC-Capabilities
vht_capab=[MAX-AMSDU-3839][SHORT-GI-80]

# Kanalbreite: 1 = 80 MHz (höhere Bandbreite)
vht_oper_chwidth=1

# Center-Frequenz für 80 MHz Kanal
vht_oper_centr_freq_seg0_idx=42

# ========================================
# Optionale Einstellungen
# ========================================

# Maximale Anzahl verbundener Clients (Standard: unbegrenzt)
# max_num_sta=20

# SSID verstecken (0 = sichtbar, 1 = versteckt)
# ignore_broadcast_ssid=0
```

**Speichern:** `Ctrl+O`, `Enter`, dann `Ctrl+X`

> **Quelle:** [GitHub - Raspberry Pi WLAN AC](https://github.com/raspberrypi/linux/issues/2619#issuecomment-410703338)

**Wichtige Anpassungen:**

1. **SSID:** Verwenden Sie den Funkrufnamen des Fahrzeugs für eindeutige Identifikation
2. **Passwort:** Ändern Sie das Beispiel-Passwort zu einem sicheren Passwort
3. **Country Code:** Passen Sie an Ihr Land an (DE/AT/CH/etc.)

**hostapd-Daemon konfigurieren:**

```bash
sudo nano /etc/default/hostapd
```

Folgende Zeile suchen und ändern (oder hinzufügen):

```bash
DAEMON_CONF="/etc/hostapd/hostapd.conf"
```

**Speichern:** `Ctrl+O`, `Enter`, dann `Ctrl+X`

## 🔄 Schritt 5: IP-Forwarding aktivieren

Für Internet-Sharing via Smartphone (USB-Tethering):

```bash
sudo nano /etc/sysctl.conf
```

Folgende Zeile auskommentieren (# entfernen):

```bash
net.ipv4.ip_forward=1
```

**Oder direkt aktivieren:**

```bash
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

## 🔥 Schritt 6: Firewall/NAT einrichten (optional)

Falls Internet-Zugang via Smartphone gewünscht:

```bash
# NAT (Network Address Translation) einrichten
sudo iptables -t nat -A POSTROUTING -o usb0 -j MASQUERADE
sudo iptables -A FORWARD -i usb0 -o br0 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i br0 -o usb0 -j ACCEPT

# Regeln speichern
sudo apt install iptables-persistent -y
sudo netfilter-persistent save
```

> **Hinweis:** `usb0` ist das Interface für USB-Tethering. Bei LTE-Sticks kann es auch `eth1` oder ähnlich sein.

## ✅ Schritt 7: System neu starten

Alle Dienste starten und testen:

```bash
sudo reboot
```

Nach dem Neustart sollte automatisch:
- Die Bridge `br0` aktiv sein
- Der DHCP-Server laufen
- Der WLAN Access Point sichtbar sein

## 🔍 Verifizierung

**1. Bridge-Status prüfen:**

```bash
brctl show
```

Erwartete Ausgabe:
```
bridge name     bridge id               STP enabled     interfaces
br0             8000.xxxxxxxxxxxx       no              eth0
                                                        wlan0
```

**2. IP-Konfiguration prüfen:**

```bash
ip addr show br0
```

Sollte `192.168.66.1/24` anzeigen.

**3. WLAN Access Point prüfen:**

```bash
sudo systemctl status hostapd
```

Status sollte `active (running)` sein.

**4. DHCP-Server prüfen:**

```bash
sudo systemctl status dnsmasq
```

Status sollte `active (running)` sein.

**5. Verbundene Clients anzeigen:**

```bash
# DHCP-Leases
cat /var/lib/misc/dnsmasq.leases

# Oder live
sudo journalctl -u dnsmasq -f
```

## 🐛 Troubleshooting

### Access Point startet nicht

```bash
# Logs prüfen
sudo journalctl -u hostapd -n 50

# Manuell testen
sudo hostapd -d /etc/hostapd/hostapd.conf
```

### Keine DHCP-Adressen werden vergeben

```bash
# dnsmasq neu starten
sudo systemctl restart dnsmasq

# Logs prüfen
sudo journalctl -u dnsmasq -n 50
```

### Bridge funktioniert nicht

```bash
# Bridge neu erstellen
sudo ip link set br0 down
sudo brctl delbr br0
sudo brctl addbr br0
sudo brctl addif br0 eth0 wlan0
sudo ip addr add 192.168.66.1/24 dev br0
sudo ip link set br0 up
```

### WLAN-Kanal ändern

Wenn Kanal 36 Probleme macht, andere 5 GHz Kanäle testen:

```bash
sudo nano /etc/hostapd/hostapd.conf
```

Ändern Sie:
```bash
channel=40  # oder 44, 48, 149, 153, 157, 161
vht_oper_centr_freq_seg0_idx=42  # Anpassen an Kanal
```

**Kanal-Mapping:**
- Kanal 36 → Center 42
- Kanal 40 → Center 42  
- Kanal 44 → Center 46
- Kanal 48 → Center 46

## 📊 Netzwerk-Topologie

```
┌──────────────────────────────────────────┐
│         Raspberry Pi (192.168.66.1)      │
│                                          │
│  ┌─────────┐    ┌──────┐    ┌─────────┐ │
│  │  br0    │◄──►│ DHCP │◄──►│  DNS    │ │
│  └────┬────┘    └──────┘    └─────────┘ │
│       │                                  │
│  ┌────┴──────────┐                       │
│  │               │                       │
│ ┌▼────┐      ┌───▼──┐                    │
│ │ eth0│      │ wlan0│                    │
│ └──┬──┘      └───┬──┘                    │
└────┼─────────────┼─────────────────────┘
     │             │
     │             │ WLAN (5 GHz)
     │             ▼
     │      ┌─────────────┐
     │      │   Tablets   │
     │      │   Drohnen   │
     │      │   Laptops   │
     │      └─────────────┘
     │
     │ Ethernet
     ▼
┌─────────────┐
│  Monitor    │
│  Switch     │
└─────────────┘
```

## 🔐 Sicherheits-Tipps

1. **Starkes WLAN-Passwort verwenden** (min. 16 Zeichen)
2. **SSID anpassen** (Funkrufname des Fahrzeugs)
3. **Country Code korrekt setzen** (regulatorische Anforderungen)
4. **Regelmäßige Updates** durchführen
5. **Nicht genutzten SSH-Zugang deaktivieren**

## 🔧 Weitere Optimierungen

### Performance-Tuning

```bash
# GPU-Memory reduzieren (kein Desktop benötigt)
echo "gpu_mem=16" | sudo tee -a /boot/config.txt

# Übertaktung für bessere Performance (optional, auf eigene Gefahr)
# echo "arm_freq=1800" | sudo tee -a /boot/config.txt
# echo "over_voltage=2" | sudo tee -a /boot/config.txt
```

### Automatische Backups

```bash
# Backup der Konfigurationen
sudo mkdir -p /root/backup
sudo cp /etc/hostapd/hostapd.conf /root/backup/
sudo cp /etc/dnsmasq.conf /root/backup/
sudo cp /etc/network/interfaces /root/backup/
```

## 📚 Weiterführende Dokumentation

- [Streaming-Setup](streaming.md) - MediaMTX Container konfigurieren
- [Hardware-Anforderungen](hardware.md) - Komponenten und Einkaufslisten
- [Übersicht](overview.md) - System-Architektur

## 🆘 Support

Bei Problemen:
1. Logs prüfen: `sudo journalctl -xe`
2. Dienste neu starten: `sudo systemctl restart hostapd dnsmasq`
3. System neu starten: `sudo reboot`