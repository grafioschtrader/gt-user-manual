---
title: "Unterschiedliche Installationsarten"
date: 2021-04-16T18:54:47+01:00
draft: false
weight: 55
archetype: "default"
---
Grafioschtrader (GT) kann für unterschiedliche Nutzungen installiert werden.

Das folgende Video beantwortet diese Fragen:
+ Repository der GT-Daten, Tabellen werden unter den Benutzer geteilt
+ Auf welcher Infrastruktur kann GT betrieben werden
+ Warum basiert GT auf JWT
+ Warum braucht es ein DNS und TLS Zertifikat
{{< youtube sF8iGgKRupE >}}

### Lokale Installation ohne DNS und SSL/TLS Zertifikate
Es ist durchaus möglich GT lokal oder in einem lokalen Netzwerk zu installieren. Software-Entwickler arbeiten meistens mit einer lokalen Version. Sie können GT auch in einem lokalen Netzwerk ohne Domain Name System (DNS) nutzen, allerdings verzichten Sie dabei auf ein SSL/TLS Zertifikat.
+ **Vorteile**: 
  + Installation ist einfacher, kein DNS und TLS Zertifikat.
  + Sicher von Angriffen ausserhalb des lokalen Netzes.
  + Auch die geteilten Daten gehören Ihnen und Sie haben die Benutzerrechte des Administratoren.
+ **Nachteile**:
  + Sie müssen alle Ihre **geteilten Daten** selber verwalten.
  + GT wird es in der **Zukunft** ermöglichen **historische Kursdaten** unter den **GT-Instanzen** zu versenden, damit reduziert sich die Abhängigkeiten zu den externen Datenquellen.
  + Eine zukünftige Smartphone-Applikation kann nicht auf Ihre GT-Instanz zugreifen.

### Installation mit DNS und SSL/TLS Zertifikate
Sie können eine **GT-Instanz** für Ihre alleinige oder allgemeine Nutzung installieren. Dabei verwenden Sie **DNS** und ein **SSL/TLS Zertifikat**.
+ **Vorteile**: 
  + Sie können von den **anderen Benutzer profitieren**, da eine gemeinsame Verwaltung der **geteilten Daten**.
  + Sie geniessen die Vorteile der GT-Weiterentwicklung bezüglich der Kommunikation zwischen den GT-Instanzen.  
+ **Nachteil**:
  + Installation ist komplizierter.

#### Selbst gehostet mit DDNS/DNS und TLS Zertifikat
Eine kleine Installation mit wenigen Benutzer läuft schon auf einem privaten [Raspberry Pi 4](//www.raspberrypi.org/products/raspberry-pi-4-model-b/). Ein TLS Zertifikat kriegen Sie bei [Let's Encrypt](//letsencrypt.org/) gratis. Kostenlose freie dynamische DNS gibt es beispielsweise bei [DuckDNS](//www.duckdns.org/). Leider ist bei vielen Internetanbietern die Upload Geschwindigkeit noch immer sehr gering, dies kann zu Verzögerungen bei externen Zugriffen auf Ihren an das Internet angeschlossenen Heim-Server führen.

Alternativ gibt es heute günstige Virtual Private Server (VPS), diese haben nicht den Nachteil von Upload Geschwindigkeiten oder einer sich ändernden IP-Adresse. Zudem müssen Sie sich auch nicht um die Firewall ihres Heimrouters kümmern.

#### Anbieter als (SaaS)
Falls Sie über die entsprechenden Computerressourcen im Internet verfügen, können Sie GT auch als **Software as a Service** (SaaS) anbieten. Dabei gibt es heute eine vielzahl von Möglichkeiten wie beispielsweise Root- und Virtual Server.

## Warum ein TLS Zertifikat
GTs **Benutzerauthentifizierung** basiert auf dem **JSON Web Token**. Für die Kommunikation im Internet ist ein **TLS Serverzertifikat** für GT notwendig, andernfalls kann keine sichere Verbindung zwischen dem Client und Server bzw. Server und Server statt finden. Zusätzlich sind dadurch Ihre sensiblen Finanzdaten geschützt.

### JSON Web Token (JWT)
Lange Zeit wurden Cookies für die Benutzerauthentifizierung im Web verwendet. Dies funktioniert für bestimmte Anwendungen sehr gut, hat aber den Nachteil, dass der Authentifizierungsvorgang einen Webbrowser benötigt. GT ist eine Single-Page-Webanwendung (SPA) die vorwiegend über ein REST-API mit dem Back-End kommuniziert. Dieses REST-API kann auch von einem nicht Webbrowser genutzt werden. Daher basiert die Authentifikation von GT auf dem **JSON Web Token** (JWT). Wer dieses **JWT** besitzt hat den Schlüssel der **Authentifizierung** und damit den **Zugang** zu den Daten, welche damit geschützt sind. Damit dieses **JWT** nicht in "falsche Hände" gerät, muss die Kommunikation im öffentlichen Netzwerk über eine **HTTPS-Verbindung** erfolgen.
