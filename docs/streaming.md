# Streaming-Setup

Container-basierte Streaming-Lösung mit MediaMTX für Live-Video-Übertragung von Drohnen und Kameras.

## 🎯 Übersicht

Das Streaming-System nutzt:
- **Podman** als Container-Runtime (leichtgewichtige Docker-Alternative)
- **MediaMTX** als Universal-Streaming-Server
- Unterstützte Protokolle: RTSP, RTMP, HLS, WebRTC

## 🔧 Vorteile von Podman

Warum Podman statt Docker?

✅ **Rootless**: Kann ohne Root-Rechte laufen  
✅ **Daemonless**: Kein permanenter Hintergrund-Dienst  
✅ **Kompatibel**: Nutzt Docker-Images und -Befehle  
✅ **Sicherer**: Bessere Isolation  
✅ **Leichtgewichtig**: Weniger Overhead

## 📦 Schritt 1: Podman installieren

```bash
sudo apt update
sudo apt install podman -y
```

**Version prüfen:**

```bash
podman --version
```

**Optional - Docker-Kompatibilität:**

```bash
# Podman als "docker" verfügbar machen
echo 'alias docker=podman' >> ~/.bashrc
source ~/.bashrc
```

source ~/.bashrc
```

## 🎥 Schritt 2: MediaMTX Container starten

[MediaMTX](https://mediamtx.org/) ist ein leistungsstarker, universeller Streaming-Server.

### Basis-Konfiguration

**Container starten:**

```bash
podman run -d \
-e MTX_RTSPTRANSPORTS=tcp \
-e MTX_WEBRTCADDITIONALHOSTS=192.168.66.1 \
-p 8554:8554 \
-p 1935:1935 \
-p 8888:8888 \
-p 8889:8889 \
-p 8890:8890/udp \
-p 8189:8189/udp \
hub.docker.com/bluenviron/mediamtx:1
```