# ExamPrep — Workstation Optimization & State Recovery Toolkit

![PowerShell](https://img.shields.io/badge/PowerShell-5.1%20%7C%207+-5391FE?style=flat-square&logo=powershell&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-Windows%2010%2F11-0078D6?style=flat-square&logo=windows&logoColor=white)
![Config](https://img.shields.io/badge/Config-JSON%20Driven-0F172A?style=flat-square)

**ExamPrep** è un modulo di automazione amministrativa in **PowerShell** progettato per preparare in modo deterministico una workstation Windows prima di sostenere esami universitari online che richiedono ambienti di **proctoring** ufficiali (es. *Safe Exam Environment / eCampus Proctor*), garantendo stabilità di rete, assenza di processi in conflitto e il ripristino completo dello stato del PC al termine della sessione.

---

## Ciclo di Vita Operativo

Il toolkit opera in **4 modalità distinte** gestite da `Invoke-ExamPrep.ps1` e dal modulo `ExamPrep/ExamPrep.psm1`:

1. **Snapshot & Backup dello Stato:** Salva la configurazione corrente dei servizi Windows, del profilo energetico attivo e delle impostazioni dell'adattatore di rete prima di applicare qualsiasi modifica.
2. **Modalità `Preparation` (Pre-Esame):**
   - Chiude automaticamente le applicazioni di background non ammesse dai software di proctoring (client di messaggistica, overlay grafici, sincronizzazioni cloud, IDE) elencate in `ExamPrep.config.json`.
   - Sospende temporaneamente i servizi di sistema ad alto impatto I/O o banda (`SysMain`, `WSearch`, `BITS`, `DoSvc`, `wuauserv`), preservando intatti tutti i servizi di sicurezza (`WinDefend`).
   - Imposta il piano energetico ad alte prestazioni e ottimizza la latenza di rete per evitare disconnessioni durante la prova.
3. **Modalità `Restore` (Post-Esame):**
   - Ripristina esattamente lo stato originale dei servizi, del profilo energetico e dei parametri di rete salvati nello snapshot iniziale.
4. **Modalità `Report` (Diagnostica):**
   - Genera un report tecnico completo (`Report_PC_Definitivo.txt`) con i dettagli hardware, sistema operativo, stato della rete e processi attivi.

---

## Struttura del Progetto

```text
prepara-pc-esami-universita/
├── ExamPrep/
│   ├── ExamPrep.psd1               # Manifesto del modulo PowerShell
│   └── ExamPrep.psm1               # Core logic (Backup, Ottimizzazione, Ripristino, Report)
├── ExamPrep.config.json            # Lista configurabile di processi, servizi e parametri
├── Invoke-ExamPrep.ps1             # Entry point CLI con auto-elevazione privilegi UAC
├── Avvia_Preparazione_Esame.bat    # Launcher rapido modalità Preparazione
├── Avvia_Ripristino.bat            # Launcher rapido modalità Ripristino
└── Avvia-Report.bat                # Launcher rapido generazione Report diagnostico
```

---

## Utilizzo da Riga di Comando

```powershell
# 1. Preparazione del PC prima dell'esame (esegue backup automatico + ottimizzazione)
.\Invoke-ExamPrep.ps1 -Mode Preparation

# 2. Ripristino completo dello stato iniziale al termine dell'esame
.\Invoke-ExamPrep.ps1 -Mode Restore

# 3. Generazione report diagnostico di sistema
.\Invoke-ExamPrep.ps1 -Mode Report
```

---

## Autore
Sviluppato da **Giancarlo Allegretti** — *Ingegneria Informatica e dell'Automazione*.
