# TransalpOS 🏍️

**Cruscotto digitale open source per Honda Transalp XL 650V (2002–2007)**

> Un progetto della community, per la community.  
> Nessun abbonamento. Nessun cloud. Solo la tua moto, i tuoi dati, il tuo viaggio.

---

## Cos'è TransalpOS

TransalpOS è un sistema operativo per cruscotto digitale progettato specificamente per la Honda Transalp XL 650V — una moto completamente analogica che merita di entrare nel XXI secolo senza perdere la sua anima.

Il progetto nasce da una domanda semplice: perché spendere 400–800€ per un navigatore commerciale che non sa nemmeno quanti giri fa il tuo motore, quando con 150€ di componenti e del buon codice open source puoi avere un cruscotto completo, integrato, e fatto su misura per la tua moto?

La risposta è TransalpOS.

---

## Cosa mostra il cruscotto

| Dato | Fonte |
|---|---|
| Velocità | Sensore Hall esistente sulla ruota |
| Giri motore (RPM) | Segnale pickup bobina (con optoisolatore) |
| Temperatura motore | Sonda NTC esistente sul motore |
| Livello carburante | Galleggiante resistivo esistente nel serbatoio |
| Pressione pneumatici | Sensori TPMS BLE aftermarket |
| Consumo istantaneo e medio | Calcolo da RPM + velocità + mappa carburante |
| Angolo di piega | IMU (accelerometro + giroscopio) |
| Navigazione GPS | Mappe offline via OsmAnd o Google Maps |
| Musica / CarPlay | Integrazione app esterne via Android |
| Dashcam | Telecamera anteriore opzionale |
| Alert e spie | Motore, riserva, temperatura, pressione gomme |

---

## Architettura del sistema

```
[Sensori analogici Transalp]
        │
        ▼
   [ESP32 — acquisizione]
   • Velocità (interrupt Hall)
   • RPM (optoisolatore + bobina)
   • Temperatura (ADC + NTC)
   • Carburante (ADC + galleggiante)
        │
        │ BLE / UART
        ▼
[Raspberry Pi 4 — cervello]         [Sensori esterni]
   • TransalpOS (Python + Web)  ◄── • TPMS BLE valvole
   • GPS u-blox NEO-M8N              • IMU BMI270
   • Navigazione offline             • Telecamera
        │
        │ HDMI + USB
        ▼
[Display Lilliput 7" IP65]
   • Touch capacitivo
   • 1000 nit — leggibile al sole
   • Montato nel cruscotto originale
```

Il **Raspberry Pi** sta nascosto dentro la carena — protetto da vibrazioni e pioggia.  
Solo **4 fili** arrivano al display sul manubrio: HDMI, USB touch, +12V, GND.  
Il **cruscotto originale della Transalp** viene preservato e usato come contenitore — nessuna modifica permanente alla moto.

---

## Stack tecnologico

| Componente | Tecnologia |
|---|---|
| Sistema operativo | Raspberry Pi OS Lite (headless) |
| Backend sensori | Python 3 + asyncio |
| Frontend cruscotto | React + WebSocket |
| Comunicazione ESP32↔Pi | BLE / UART seriale |
| Navigazione | OsmAnd (offline) |
| GPS | u-blox NEO-M8N — multi-costellazione |
| IMU | BMI270 — ottimizzato per vibrazioni |

---

## Roadmap

### Fase 1 — Simulazione (in corso)
- [x] UI del cruscotto — layout e dati simulati
- [ ] Server Python con dati mock
- [ ] Repository GitHub pubblico
- [ ] Demo video per la community

### Fase 2 — Prototipo hardware
- [ ] ESP32 + acquisizione sensori reali
- [ ] Integrazione GPS
- [ ] Test su banco con Transalp 650V reale
- [ ] Prima release installabile

### Fase 3 — Community release
- [ ] Guida di installazione completa
- [ ] Schemi elettrici in formato KiCad
- [ ] File STL per supporti stampabili in 3D
- [ ] Supporto per altri anni/modelli Transalp

---

## Hardware necessario

| Componente | Prezzo indicativo |
|---|---|
| Raspberry Pi 4 (2GB) | ~45€ |
| ESP32 dev board | ~8€ |
| Display Lilliput 7" IP65 HDMI touch | ~100€ |
| GPS u-blox NEO-M8N | ~20€ |
| IMU BMI270 (Adafruit) | ~12€ |
| Sensori TPMS BLE (2x) | ~20€ |
| DC-DC 12V→5V + cablaggi | ~15€ |
| **Totale stimato** | **~220€** |

*Risparmio rispetto a soluzioni commerciali equivalenti: 300–600€*

---

## Come contribuire

TransalpOS è un progetto aperto. Ogni contributo è benvenuto:

- **Hai una Transalp 650V?** Puoi testare i sensori e riportare i valori reali
- **Sai programmare Python o React?** Guarda le issue aperte
- **Sei un elettronico?** Aiutaci a raffinare gli schemi di interfacciamento
- **Parli inglese/tedesco/spagnolo?** Traduci la documentazione

Apri una issue, manda una pull request, o scrivici nei forum della community.

---

## Community e supporto

Il progetto nasce dalla community italiana degli appassionati di Honda Transalp.  
Se hai una Transalp e vuoi essere tra i primi a installare TransalpOS, apri una issue con l'etichetta `beta-tester`.

- Forum: *link in arrivo*
- GoFundMe: *link in arrivo — per finanziare il prototipo hardware*

---

## Licenza

TransalpOS è rilasciato sotto licenza **MIT**.  
Puoi usarlo, modificarlo, distribuirlo liberamente — anche commercialmente.  
L'unica cosa che chiediamo: se migliori qualcosa, condividi con la community.

---

## Disclaimer

TransalpOS è un progetto hobbistico e sperimentale.  
Non è certificato per uso su strada. Installazione a proprio rischio.  
Non modificare i sistemi di sicurezza della moto (freni, luci, ABS).  
Rispetta sempre il codice della strada.

---

*Fatto con passione da motociclisti, per motociclisti.*  
*"Non è un F-35. Deve mostrarti i giri motore in modo figo mentre attraversi il Passo della Cisa."*
