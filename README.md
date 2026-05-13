# FileShift

Convertitore di file online gratuito — immagini, audio e video elaborati direttamente nel browser tramite Canvas API e FFmpeg.wasm. Nessun server, nessun upload, privacy totale.

## Struttura del progetto

```
fileshift/
├── index.html        # Homepage + converter funzionante
├── 404.html          # Pagina errore personalizzata
├── robots.txt        # Istruzioni per i motori di ricerca
├── sitemap.xml       # Mappa del sito per Google
├── _headers          # Header HTTP (Netlify/Cloudflare Pages)
└── README.md         # Questo file
```

## Deploy su GitHub Pages

### 1. Crea il repository
```bash
git init
git add .
git commit -m "primo commit"
git branch -M main
git remote add origin https://github.com/TUO-USERNAME/fileshift.git
git push -u origin main
```

### 2. Attiva GitHub Pages
- Vai su **Settings → Pages**
- Source: **Deploy from a branch**
- Branch: **main** → cartella **/ (root)**
- Salva — il sito va live su `https://TUO-USERNAME.github.io/fileshift`

### 3. Dominio personalizzato (opzionale)
- Compra il dominio su Porkbun/Namecheap
- Su Cloudflare: aggiungi il sito, copia i nameserver
- Su GitHub Pages → Custom domain: inserisci `fileshift.io`
- Su Cloudflare: aggiungi record CNAME `www` → `TUO-USERNAME.github.io`
- Aggiungi 4 record A per l'apex domain:
  ```
  185.199.108.153
  185.199.109.153
  185.199.110.153
  185.199.111.153
  ```
- Spunta **Enforce HTTPS** su GitHub Pages

### ⚠️ Nota importante su FFmpeg.wasm
FFmpeg.wasm richiede gli header `Cross-Origin-Opener-Policy` e `Cross-Origin-Embedder-Policy` per funzionare con SharedArrayBuffer. **GitHub Pages non supporta header HTTP personalizzati**, quindi la conversione audio/video potrebbe non funzionare.

**Soluzione consigliata:** usa **Cloudflare Pages** invece di GitHub Pages — è gratuito, supporta il file `_headers` incluso in questo progetto, e ha CDN globale.

#### Deploy su Cloudflare Pages (consigliato)
1. Vai su [pages.cloudflare.com](https://pages.cloudflare.com)
2. Connetti il tuo repository GitHub
3. Build settings: nessuno (sito statico puro)
4. Deploy — gli header `_headers` vengono applicati automaticamente

## Come aggiungere nuove pagine di conversione (SEO)

Per ogni conversione crea una cartella con un `index.html`:
```
converti/
  png-in-pdf/
    index.html
  mp4-in-mp3/
    index.html
  ...
```

Ogni pagina deve avere:
- Title e meta description specifici per quella conversione
- H1 con la keyword esatta ("Converti PNG in PDF online gratis")
- Il converter pre-impostato sul formato corretto
- Testo descrittivo di almeno 300 parole
- FAQ specifiche

## Tecnologie usate

| Funzione | Tecnologia | Costo |
|---|---|---|
| Conversione immagini | Canvas API (nativa browser) | €0 |
| Conversione audio/video | FFmpeg.wasm 0.11.6 | €0 |
| PDF da immagine | jsPDF 2.5.1 | €0 |
| Hosting | GitHub Pages / Cloudflare Pages | €0 |
| CDN + SSL | Cloudflare | €0 |

## Aggiungere Google Analytics

Aggiungi prima del `</head>` in `index.html`:
```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

## Aggiungere AdSense (quando approvato)

Aggiungi nel `<head>`:
```html
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-XXXXXXXXXXXXXXXX" crossorigin="anonymous"></script>
```

Poi inserisci i blocchi `<ins class="adsbygoogle">` nei punti desiderati.

## Inviare la sitemap a Google

1. Vai su [Google Search Console](https://search.google.com/search-console)
2. Aggiungi il tuo dominio
3. Verifica la proprietà (metodo DNS con Cloudflare è il più semplice)
4. Vai su **Sitemaps → Aggiungi sitemap**
5. Inserisci: `https://fileshift.io/sitemap.xml`
