# Rapportino — App Ten Solutions

App Android per compilare il rapportino di lavoro giornaliero e generare il file Excel
(`nome.cognome.ddmmyy.xlsx`) identico al modello Ten Solutions, con logo, colori variante/urgente,
multi-cantiere e colleghi. Funziona **offline** (ExcelJS e logo sono inclusi in locale).

## Come ottenere l'APK (build in cloud, senza PC)

La compilazione avviene su **GitHub Actions**: non serve installare Android SDK, Gradle o Java.

1. Crea un repository su GitHub e carica **tutti i file di questa cartella** (senza `node_modules`
   e senza `android/`, che vengono generati in automatico).
2. Ad ogni push sul branch `main`/`master` — oppure lanciandolo a mano da **Actions →
   Build APK → Run workflow** — parte la build.
3. A fine build apri il workflow, sezione **Artifacts**, e scarica `rapportino-apk`
   (contiene `app-debug.apk`).
4. Sul telefono installa l'APK abilitando **"Installa app da origini sconosciute"**.

## Struttura

```
www/
  index.html        L'app (form + generazione xlsx)
  exceljs.min.js    Libreria ExcelJS in locale (offline)
capacitor.config.json
package.json
.github/workflows/build.yml
```

`android/` NON è incluso: lo genera il workflow con `npx cap add android`.

## Sviluppo / test in locale (opzionale, da PC)

```bash
npm install
npx cap add android
npx cap sync android
npx cap open android      # apre Android Studio
```

Per modificare l'app basta editare `www/index.html`; poi `npx cap sync android` e ricompilare.
Su GitHub è sufficiente ricaricare `www/index.html` e lasciar ripartire il workflow.

## Invio

Su Android l'app scrive il file e apre il **foglio di condivisione nativo** con l'xlsx già allegato:
scegli WhatsApp, email o Drive. Il destinatario email impostato nelle impostazioni viene usato nel
testo/oggetto; per l'invio email **diretto al destinatario in un tap** con allegato serve aggiungere
un plugin mail-composer (vedi sotto).

## Note / possibili estensioni

- **Invio email diretto al destinatario**: aggiungere un plugin di mail composer e usarlo al posto di
  `Share` quando è impostato il destinatario.
- **Icona app**: attualmente icona di default Capacitor. Si può generare dall'immagine del logo con
  `@capacitor/assets`.
- **appId**: `it.iw1fzr.rapportino` — modificabile in `capacitor.config.json`.
