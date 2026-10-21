# OZ LED: Rechtstexte veröffentlichen

`index.html` ist die zweisprachige Seite für Datenschutz und Impressum, `support.html` die
Hilfeseite, die Apple als Support-URL verlangt. Beide kommen ohne JavaScript, externe
Schriften, Analyse und eigene Cookies aus. Aufbau und Rechtstexte folgen VAYA
(`/Users/denis/Documents/VAYA/docs/`), Inhalt und Aussehen sind auf OZ LED angepasst
(dunkel, Akzent `#8A5CFF`).

## Wo die Seiten liegen

Dieser Ordner ist die **Quelle**. Ausgeliefert werden die Seiten aus einem zweiten,
öffentlichen Repo, genau wie bei VAYA (`vaya-cloud/vaya-privacy`):

| | |
|---|---|
| Quelle | `docs/web/` in diesem Projekt |
| Veröffentlichung | Repo `vaya-cloud/oz-led-privacy`, öffentlich, GitHub Pages |
| Datenschutz-URL | `https://vaya-cloud.github.io/oz-led-privacy/` |
| Support-URL | `https://vaya-cloud.github.io/oz-led-privacy/support.html` |

Das Code-Repo (`vaya-cloud/OZLED`) bleibt privat. Das Pages-Repo enthält nur diese zwei
HTML-Dateien und sonst nichts.

## Einmalig einrichten

1. Auf GitHub in der Organisation **vaya-cloud** ein neues **öffentliches** Repo
   `oz-led-privacy` anlegen. GitHub Pages ist mit dem kostenlosen Plan nur für öffentliche
   Repos verfügbar.
2. `index.html` und `support.html` aus diesem Ordner in die **Wurzel** des Repos kopieren
   und auf `main` pushen:

   ```sh
   git clone https://github.com/vaya-cloud/oz-led-privacy.git /tmp/oz-led-privacy
   cp "/Users/denis/Scheinwerfer App/docs/web/index.html" \
      "/Users/denis/Scheinwerfer App/docs/web/support.html" /tmp/oz-led-privacy/
   cd /tmp/oz-led-privacy && git add . && git commit -m "Datenschutz, Impressum, Hilfe" && git push
   ```

3. Auf GitHub im Repo **Settings → Pages** öffnen.
4. Unter *Build and deployment* die Quelle **Deploy from a branch** wählen, Branch **main**,
   Ordner **/ (root)**, dann **Save**.
5. Nach ein bis zwei Minuten beide URLs aufrufen und alle Sprungziele prüfen: Datenschutz,
   Impressum, Privacy, Legal notice, und den Wechsel zwischen den beiden Seiten.

## Wo die URLs eingetragen werden

**App Store Connect** (für jede Lokalisierung, DE und EN):

- App-Informationen → **Datenschutzrichtlinie-URL**: `https://vaya-cloud.github.io/oz-led-privacy/`
- App-Informationen → **Support-URL**: `https://vaya-cloud.github.io/oz-led-privacy/support.html`
- Marketing-URL: optional, leer lassen.

**In der App** (Pflicht nach Guideline 5.1.1(i): ein Link, nicht nur Text), an einer
Stelle gesammelt wie `LegalContent` in VAYA:

```swift
enum LegalContent {
    static let name = "Denis Liebscher"
    static let street = "Hamburger Str. 17"
    static let city = "04129 Leipzig"
    static let email = "help.vaya@web.de"
    static let privacyPolicy = "https://vaya-cloud.github.io/oz-led-privacy/"
    static let privacyPolicyDisplay = "vaya-cloud.github.io/oz-led-privacy"
    static let support = "https://vaya-cloud.github.io/oz-led-privacy/support.html"
}
```

Dieselben Werte stehen in `docs/appstore/appstore-eintraege.md` §7–§8 und in
`App/OZLED/Einstellungen/RechtlichesView.swift` (`LegalContent`).

## Die Mailadresse steht absichtlich zerlegt

Auf beiden Seiten steht `help.vaya (at) web (punkt) de` statt eines `mailto:`-Links. Das
ist kein Versehen: Bei VAYA hat die maschinenlesbare Adresse auf der öffentlichen Seite
ASO-Spam angezogen. In der App bleibt die Adresse ein `mailto:`-Link
(`LegalContent.email`), dort liest sie kein Sammler aus.

## Nach jeder Änderung

- Hier ändern, dann **beide** Dateien in `oz-led-privacy` kopieren und pushen. Sonst zeigt
  die öffentliche Seite etwas anderes als die App; bei VAYA ist genau das mit der
  Mailadresse passiert.
- Das Datum „Stand: …“ oben in beiden Dateien anpassen.
- Vor jedem Release gegenprüfen, dass vier Dinge dasselbe sagen: diese Seiten,
  `LegalContent` und der Datentext in der App, `PrivacyInfo.xcprivacy` und die Antwort
  „Data Not Collected“ in App Store Connect. Kommt eine Funktion mit Netzwerk, Kamera
  oder anderen Daten dazu, müssen Seite, Manifest und
  App Privacy **vor** dem Upload angepasst werden.

## Was die Seiten behaupten und worauf das beruht

Die Seiten beschreiben den Stand vom 20. Oktober 2026 (M6, LEDCAR-02):

- **Hardware:** eine Bluetooth-LE-LED-Steuerbox (LEDCAR-02-Bauart), mit der Zündung immer an,
  auch ohne Handy; optional zwei Relais-Boxen (DSD), die den Blinker zwischen LED-Leiste und
  Original-Birne umschalten („Nur Originalblinker“). Es gibt keine Lichtmodi „Stock“/„LED-Band“
  mehr.
- **Bedienung:** ein Hauptbildschirm, Einstellungen über das Zahnrad; Start-Animation (51
  eingebaut, gezeichnete Vorschau) und Show-Wechsel laufen in der Steuerbox.
- **Bluetooth LE** nur zur eigenen Steuerbox und zu den Relais-Boxen; `bluetooth-central` im
  Hintergrund nur zum Wiederverbinden bei Zündung.
- **Mikrofon** nur während des Effekts „Musik“, nur im Vordergrund, Auswertung im
  Arbeitsspeicher, nichts wird gespeichert oder gesendet. Es ist das **iPhone**-Mikrofon; die
  Takt-Quelle „Steuerbox“ gibt es nur in der versteckten Spotify-Beta und wird nicht beworben.
- **Spotify:** Das offizielle Spotify iOS SDK ist eingebunden, die Funktion aber standardmäßig
  ausgeblendet (Beta). Die Datenschutzseite nennt das ehrlich; Store-Texte und Prüfhinweise
  nennen Spotify nicht, solange es versteckt ist.
- **Siri, Kurzbefehle, Widgets (Home, Sperrbildschirm) und Kontrollzentrum** laufen lokal über
  App Intents/WidgetKit.
- SwiftData und UserDefaults liegen in der App Group `group.de.ozled.app` auf dem Gerät.
- Kein Konto, kein Server, kein eigener Netzwerkcode, keine Analyse, keine Werbung, keine
  Kamera.

Ändert sich davon etwas, stimmt die Seite nicht mehr.

## Veröffentlichen (Stand 20.10.)

Denis hat am 20.10. freigegeben, die aktualisierten Seiten `index.html` und `support.html`
ins Pages-Repo `vaya-cloud/oz-led-privacy` hochzuladen (nur diese zwei Dateien). Danach beide
URLs aufrufen und die Sprungziele prüfen.
