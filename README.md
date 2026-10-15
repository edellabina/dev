# INGV Waveform Viewer — Web-Based Seismic Signal Fetcher 

[https://edellabina.github.io/dev/lav9.html](https://edellabina.github.io/dev/lav9.html)

A lightweight, purely client-side web application designed to request, decode, and plot seismic waveform data directly from the **INGV (Istituto Nazionale di Geofisica e Vulcanologia)** FSDN Web Services without relying on Python or backend processing.

---

## 🇮🇹 Italiano

### Panoramica
Questa applicazione web permette di visualizzare i segnali sismici forniti dall'INGV direttamente dal browser. A differenza dei tradizionali approcci sismologici che richiedono ambienti Python (con librerie come `ObsPy`) o software dedicati, questo progetto gestisce l'intero ciclo di vita del dato — **richiesta HTTP, decodifica del formato binario miniSEED e rendering grafico** — completamente lato client tramite HTML5 e JavaScript puro.

### Architettura e Funzionamento

```
[ Input Utente ] ──> [ Richiesta HTTP Fetch ] ──> [ INGV FSDN Web Service ]
                                                           │
                                             (Restituisce file .mseed binario)
                                                           │
[ Grafico Canvas HTML5 ] <── [ Disegno 2D ] <── [ Decodifica Binary/Steim ]
```

1. **Richiesta Parametrizzata:** L'utente seleziona la stazione (es. `CEL`), la rete (`IV`), la componente del canale (`HHZ`, `EHZ`, ecc.) e l'intervallo temporale. JavaScript formatta l'URL di interrogazione per il servizio `dataselect` dell'INGV.
2. **Download del dato binario:** L'API nativa `fetch()` scarica i dati grezzi in formato **miniSEED** (`.mseed`) restituendo un `ArrayBuffer` in memoria.
3. **Decodifica Client-Side (miniSEED/Steim):** Tramite `DataView` e `TypedArrays`, il codice analizza i record binari, decomprime le serie temporali (algoritmi di compressione Steim1/Steim2) ed elide l'offset DC (centrando il segnale sullo zero).
4. **Rendering su HTML5 `<canvas>`:** L'ampiezza (*digital counts*) e il tempo (secondi) vengono mappati in pixel e disegnati ad alte prestazioni direttamente sulla GPU del dispositivo tramite l'API 2D di Canvas.

---

## 🇬🇧 English

### Overview
This web application allows users to fetch and visualize seismic signals provided by INGV directly inside any web browser. Unlike traditional seismological workflows that require Python environments (such as `ObsPy`) or specialized software, this project executes the entire pipeline — **HTTP request, binary miniSEED decoding, and plotting** — purely on the client side using HTML5 and vanilla JavaScript.

### Architecture & How It Works

```
[ User Input ] ──> [ HTTP Fetch Request ] ──> [ INGV FSDN Web Service ]
                                                        │
                                            (Returns binary .mseed file)
                                                        │
[ HTML5 Canvas Chart ] <── [ 2D Rendering ] <── [ Binary/Steim Decoding ]
```

1. **Parameterized Query:** The user selects the network (`IV`), station code (e.g., `CEL`), channel component (`HHZ`, `EHZ`, etc.), and time frame. JavaScript formats the request URL for the INGV `dataselect` web service.
2. **Binary Data Retrieval:** The native `fetch()` API retrieves raw **miniSEED** (`.mseed`) data as an in-memory `ArrayBuffer`.
3. **Client-Side Decoding (miniSEED/Steim):** Using `DataView` and `TypedArrays`, JavaScript parses the binary headers, decompresses the time series samples (Steim1/Steim2 decompression algorithms), and removes the DC offset to center the waveform at zero.
4. **HTML5 `<canvas>` Rendering:** Signal amplitudes (*digital counts*) and timestamps are mapped onto canvas pixels and rendered smoothly via the browser's GPU using the Canvas 2D Context API.

---

## 🛠️ Key Technical Features / Caratteristiche Tecniche

* **Zero Dependencies:** No Python, Conda, or heavy external charting frameworks required.
* **Portable:** A single client-side script executable on any modern desktop or mobile browser.
* **High Performance:** GPU-accelerated rendering using standard HTML5 Canvas.
* **Open Data Integration:** Directly queries standard FSDN/EIDA Web Services provided by INGV.
