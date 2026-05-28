# Netwerkontwerp Provinciehuis

**Afstudeeropdracht — ICT Tribe Almere / SLTN Group B.V. — mei 2026**

---

## Projectomschrijving

Volledig uitgewerkt netwerkontwerp voor een fictief overheidsgebouw: het Provinciehuis.
De opdracht is verstrekt door SLTN Group B.V. als onderdeel van het ICT Tribe Almere afstudeerprogramma.

| | |
|---|---|
| **Gebouw** | 6 etages (begane grond + etages 1 t/m 5) + serverruimte |
| **Gebruikers** | ~900 medewerkers (bekabeld + WiFi) |
| **Servers** | ~30 (grotendeels VMs: Windows, Linux, appliances) |
| **Projectduur** | Mei 2026 (één projectweek) |

> Dit is een simulatieopdracht. De klant en locatie zijn fictief.

---

## Architectuur

Two-tier topologie zonder aparte distributielaag.

- **Firewall** — HA-cluster (actief/passief), SD-WAN met dubbele ISP + Diginetwerk
- **Core** — Twee L3 core switches (Core-A / Core-B) met HSRP gateway-redundantie
- **Access** — Twee netwerkkasten per etage (NK-A / NK-B), elk met 2× 48-poorts GbE + 1× 24-poorts PoE switch
- **WLAN** — WPA3-Enterprise (medewerkers) + WPA3-Personal met captive portal (gasten)
- **VLAN-segmentatie** — 802.1X poortauthenticatie gekoppeld aan centrale identiteitsdienst
- **DHCP/DNS** — Verzorgd door de firewall per VLAN, met interne DNS-server in de serverruimte
- **Simulatie** — EVE-NG lab; HSRP en inter-VLAN routing gevalideerd

---

## Opgeleverde documenten

| Bestand | Omschrijving |
|---|---|
| `docs/Eindrapport.docx` | Volledig eindrapport (architectuur, VLAN-plan, hardware, risico, validatie) |
| `docs/Projectbrief_v2.docx` | Projectbrief (scope, stakeholders, aanpak) |
| `docs/Risicoanalyse_v2.docx` | Risicoanalyse |
| `docs/Validatierapport_v2.docx` | Validatierapport (EVE-NG testresultaten) |
| `netwerk/IP_Plan_v2.xlsx` | Volledig IP-plan (subnetten, VLANs, patchlijsten) |
| `netwerk/Concept_Budget.xlsx` | CAPEX / OPEX / TCO-overzicht |
| `netwerk/Netwerktopologie.html` | Interactief netwerktopologiediagram |
| `planning/Projectplanning_v2.xlsx` | Projectplanning |
| `presentatie/Eindpresentatie_v4.pptx` | Eindpresentatie |

---

## Ontwerpkeuzes

- ISP-aansluitingen termineren op de core switches via VLAN 13/14 en worden als trunk naar beide firewalls gedistribueerd — niet direct op de firewall — zodat SD-WAN failover via zowel FGT-A als FGT-B mogelijk is
- Diginetwerk (overheidsinterconnect via Logius) is een logisch SD-WAN-lid, geen aparte fysieke lijn
- IP-schema: het tweede octet correspondeert met de VLAN-functie (bijv. `10.100.x.x` = medewerkers, `10.10.x.x` = beheer)
- Geen subnetten per etage; segmentatie verloopt uitsluitend via VLAN-grenzen met identiteitsgebaseerde toewijzing via Entra ID
- EVE-NG community-limiet (9 gelijktijdige nodes) maakte volledige simulatie onpraktisch; topologie is bewaard voor documentatiedoeleinden

---

## Team

| Naam | LinkedIn |
|---|---|
| Bastiaan van Oorde | [linkedin.com/in/colorcoded](https://www.linkedin.com/in/colorcoded/) |
| Danny Peek | [linkedin.com/in/dannypeek](https://www.linkedin.com/in/dannypeek/) |
| Robert Post | [linkedin.com/in/rm-post](https://www.linkedin.com/in/rm-post/) |
| Shervino Hodge | [linkedin.com/in/shervino-hodge](https://www.linkedin.com/in/shervino-hodge/) |
| Symon Senior | [linkedin.com/in/symon-senior-22a608364](https://www.linkedin.com/in/symon-senior-22a608364/) |

---

## Gebruikte tools

- **EVE-NG** — netwerksimulatie (IOL L2/L3, FortiGate vVM)
- **Cisco IOL** — switch- en routeremulatie
- **FortiGate** — firewall- en SD-WAN-simulatie
- **Microsoft Excel** — IP-plan, budget, projectplanning
- **Claude (Anthropic)** — documentatie, ontwerpbeslissingen en technische uitwerking
