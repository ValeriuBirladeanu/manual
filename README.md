# WWW BPAY.md - UI Automation Tests

## Descriere Generală
Acest proiect conține o suită de teste automate de User Interface (UI) pentru aplicația web BPAY.md avand scopul său principal de a asigura funcționalitatea corectă a proceselor cheie ale aplicației.
Testele sunt construite folosind **Playwright**, un **framework** modern pentru automatizarea browserelor, și **limbajul de programare JavaScript**.

## Tehnologii Utilizate
Proiectul este construit folosind următoarele tehnologii și biblioteci cheie:
- **Node.js** – mediu de execuție JavaScript.
- **npm** – manager de pachete.
- **Playwright (`@playwright/test`)** – framework de testare UI.
- **`@faker-js/faker`** – generare date random (nume, email, etc).
- **`dotenv`** – gestionare variabile de mediu.
- **`mysql2`** – interacțiune cu baza de date MySQL.
- **`redis`** – extragere OTP din loguri (email/SMS).
- **Git** – sistem de control al versiunilor..
- **Docker** – pentru rularea serviciilor locale (MySQL, Redis, Nginx, www, xapi) pentru proiectul supus automatizării.

## Pre-condiții (Prerequisites)
Pentru a rula acest proiect local, trebuie să ai instalate următoarele:
* **Node.js**: versiunea recomandată este `v20.x` (conform `@types/node` ^24.0.8, dar asigură-te că folosești o versiune compatibilă cu `^24.0.8`, ideal ultima LTS).
* **npm**: vine la pachet cu instalarea Node.js.
* **Git**: pentru a clona depozitul.
* **Docker**: pentru servicii locale (MySQL, Redis, Nginx, backend etc)

## Instalare și Configurare
Urmează acești pași pentru a configura și rula proiectul pe mașina ta locală:
1.  **Clonează depozitul:**
     ```bash
    git clone git@git.bpay.md:testing/testing.git #Folosește link-ul tău SSH
       sau
    git clone https://git.bpay.md/testing/testing.git   # Folosește link-ul tău HTTPS
    ```
    ```bash
    cd testing # Navighează în directorul proiectului
    ```
    ```bash
    git pull origin master #(Opțional) Actualizează repo-ul local

2.  **Instalează dependențele proiectului:**
    ```bash
    npm install
    ```
3.  **Configurează variabilele de mediu:**
    Creează un fișier numit `.env` în rădăcina proiectului. Acest fișier va conține credențialele bazei de date și alte variabile de configurare esențiale pentru rularea testelor.
   
4.  **Ruleaza Docker):**
    Proiectul utilizează Docker Compose pentru a gestiona serviciile locale necesare, inclusiv baza de date MySQL și serviciul Redis.

    ```bash
    docker-compose -f ./docker/docker-compose.yaml up
    ```
    Această comandă va construi și porni containerele definite în `docker/docker-compose.yaml` în fundal. 

## Rularea Testelor
Poți rula testele folosind scripturile predefinite din `package.json` sau direct comenzile Playwright cu `npx`.
* **Rulează toate testele (mod headless - implicit):**
    ```bash
    npm test
    # sau direct:
    # npx playwright test
    ```

* **Rulează toate testele (mod vizibil - headed):**
    ```bash
    npm run test:headed
    # sau direct:
    # npx playwright test --headed
    ```

* **Rulează testele în modul debug (cu UI Inspector și VS Code Debugger):**
    ```bash
    npm run test:debug
    # sau direct:
    # npx playwright test --debug
    ```

* **Deschide interfața Playwright UI (pentru debugging interactiv și vizualizare):**
    ```bash
    npm run test:ui
    # sau direct:
    # npx playwright show-report --ui
    ```

* **Rulează testele doar în browserul Chromium:**
    ```bash
    npm run test:chromium
    # sau direct:
    # npx playwright test --project=chromium
    ```

* **Rulează testele doar în browserul Firefox:**
    ```bash
    npm run test:firefox
    # sau direct:
    # npx playwright test --project=firefox
    ```

* **Pentru a vizualiza raportul HTML al rezultatelor testelor:**
    ```bash
    npx playwright show-report
    ```

## Structura Proiectului
* `tests/`: Acest director conține toate fișierele de testare `.spec.js`. Fiecare fișier `.spec.js` reprezintă, de obicei, o suită de teste pentru o anumită funcționalitate a aplicației.
* `pages/`: Aici sunt stocate **Page Object Models (POMs)**. Un Page Object este o clasă care reprezintă o pagină (sau un modul/componentă) specifică a aplicației web. Acestea încapsulează selectorii HTML și acțiunile specifice acelei pagini, îmbunătățind semnificativ lizibilitatea, mentenabilitatea și reutilizabilitatea codului de testare.
* `utils/`: Acest director conține fișiere utilitare (helper functions) și module de suport si funcții generice. 
* `playwright.config.js`: Fișierul principal de configurare pentru Playwright. Aici sunt definite setările globale pentru rularea testelor (ex: browserele țintă, configurari pentru executia testelor, cnfigurari pentru rapoarte etc).
* `.env`: Fișierul pentru variabilele de mediu. **Acest fișier nu trebuie niciodată versionat (comitat în Git) din motive de securitate.**
* `docker/`: Conține fișierele Docker (ex: `docker-compose.yaml`, `Dockerfile` pentru servicii precum `www.bpay.md` și `xapi.bpay.md`) necesare pentru a rula și configura serviciile locale.
---
