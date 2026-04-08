# 💊 MediSchrank

Private PWA zur Organisation deines Medikamentenschranks.  
Läuft vollständig offline auf iPhone, iPad und Mac (Safari).

---

## Features

- **Verpackung fotografieren** – KI (Claude API) liest Name, PZN, MHD und Menge automatisch aus
- **Barcode-Scanner** – Foto der Verpackung → EAN wird automatisch erkannt (html5-qrcode / ZXing)
- **OpenFoodFacts-Lookup** – Produktname, Hersteller und Menge werden automatisch befüllt
- **MHD-Überwachung** – farbliche Ampel + iOS Shortcuts Workaround für Benachrichtigungen
- **Offline-First** – IndexedDB, funktioniert ohne Internet
- **iCloud Drive Sync** – Export per iOS Share Sheet → „In Dateien sichern" → iCloud Drive
- **Biometrie-Sperre** – Face ID / Touch ID via WebAuthn (optional)
- **Dark / Light Mode** – manuell oder automatisch nach Systemeinstellung
- **Safari-optimiert** – kein Zoom, Safe Areas, `maximum-scale=1`

---

## Deployment (GitHub Pages)

### Repository-Struktur

```
/
├── index.html        ← App (alles in einer Datei, html5-qrcode inline)
├── manifest.json     ← PWA Manifest
└── README.md
```

### Schritt für Schritt

1. Alle Dateien in dieses Repository laden (über GitHub Web-Interface oder git)
2. **Settings → Pages → Branch:** `Hauptdarsteller` / `/ (root)` → Save
3. Nach ~1 Minute ist die App unter `https://[username].github.io/[repo]/` verfügbar
4. Auf iPhone/iPad: URL in Safari öffnen → Teilen → **„Zum Home-Bildschirm"**

> ⚠️ WebAuthn (Face ID) funktioniert **nur** über HTTPS – also nur über GitHub Pages, nicht über `file://`

---

## Technischer Stack

| Bereich | Lösung |
|---|---|
| Framework | Vanilla JS (kein Build-Tool nötig) |
| Speicher | IndexedDB (Medikamente) + localStorage (Settings) |
| Barcode | html5-qrcode 2.3.8 (inline) |
| KI-Analyse | Claude API (Verpackungsfoto → alle Felder) |
| Offline | Service Worker deaktiviert – IndexedDB übernimmt Persistenz |
| Auth | WebAuthn / Passkey (platform authenticator) |
| Produktdaten | OpenFoodFacts API (kostenlos, kein API-Key) |

---

## iOS / Safari Besonderheiten

- **Kein `BarcodeDetector`** auf iOS → Foto-basierter Scan via `input[capture=environment]`
- **Kein Background Push** → iOS Shortcuts Workaround (Setup-Anleitung in der App unter Einstellungen)
- **WebAuthn** erst ab iOS 16 + muss als Home Screen App installiert sein
- **IndexedDB** wird bei „Website-Daten löschen" geleert → regelmäßig exportieren!

---

## Datenschutz

- Keine Daten verlassen das Gerät (außer OpenFoodFacts-Lookup per Barcode und Claude API bei Foto-Analyse)
- Claude API Key wird nur lokal im localStorage gespeichert, nie übertragen
- Kein Account, kein Server, kein Tracking
