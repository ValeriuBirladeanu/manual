# BPAY.md UI Automation Tests

## Descriere Generală

Acest proiect conține o suită de teste automate de User Interface (UI) pentru aplicația web [BPAY.md](https://www.bpay.md/). Scopul său principal este de a asigura funcționalitatea corectă a proceselor cheie ale aplicației.

Testele sunt construite folosind **Playwright**, un **framework** modern pentru automatizarea browserelor, și **limbajul de programare JavaScript**.

## Tehnologii Utilizate

Proiectul este construit folosind următoarele tehnologii și biblioteci cheie:

* **Node.js**: Mediu de execuție JavaScript.
* **npm**: Manager de pachete pentru Node.js.
* **Playwright (`@playwright/test`)**: Cadrul principal pentru automatizarea testelor UI în diferite browsere (Chromium, Firefox).
* **`@faker-js/faker`**: Pentru generarea de date de test realiste și unice (ex: nume, adrese de email, numere de telefon etc).
* **`dotenv`**: Pentru gestionarea și încărcarea securizată a variabilelor de mediu din fișierul `.env`.
* **`mysql2`**: Client robust pentru interacțiunea cu baza de date MySQL.
* **`redis`**: Utilizat pentru a extrage codurile OTP (One-Time Password) pentru email și SMS, esențial pentru scenariile de verificare. 
* **Git**: Sistem de control al versiunilor.

## Pre-condiții (Prerequisites)

Pentru a rula acest proiect local, trebuie să ai instalate următoarele:

* **Node.js**: Versiunea recomandată este `v20.x` (conform `@types/node` ^24.0.8, dar asigură-te că folosești o versiune compatibilă cu `^24.0.8`, ideal ultima LTS).
* **npm**: Vine la pachet cu instalarea Node.js.
* **Git**: Pentru a clona depozitul.
* **Docker Desktop**: Necesit pentru a porni și gestiona serviciile locale (baza de date MySQL) via Docker Compose.

## Instalare și Configurare

Urmează acești pași pentru a configura și rula proiectul pe mașina ta locală:

1.  **Clonează depozitul:**
    ```bash
    git clone [https://git.bpay.md/testing/testing.git](https://git.bpay.md/testing/testing.git) # Folosește link-ul tău HTTPS sau SSH
    cd testing # Navighează în directorul proiectului
    ```

2.  **Instalează dependențele proiectului:**
    ```bash
    npm install
    # sau pentru o instalare curată, preferată în CI/CD:
    # npm ci
    ```

3.  **Configurează variabilele de mediu:**
    Creează un fișier numit `.env` în rădăcina proiectului. Acest fișier va conține credențialele bazei de date și alte variabile de configurare esențiale pentru rularea testelor.

    Exemplu de conținut pentru `.env`:
    ```dotenv
    # Configurări Bază de Date
    DB_HOST=192.168.115.4 # Sau IP-ul/host-ul serverului bazei de date
    DB_USER=valeriu_birladeanu_rw
    DB_PASSWORD=61ZCjVJzN-7BXchTQ
    DB_NAME=bpaydb2
    DB_USERS_REGISTRATIONS_TABLE=bpaydb2.bp_UsersRegistrations
    # URL-ul aplicației web de testat
    BASE_URL=[https://www.bpay.md](https://www.bpay.md)
    # Configurări Redis (dacă este cazul)
    REDIS_HOST=localhost
    REDIS_PORT=6379
    ```
    **ATENȚIE:** Fișierul `.env` conține informații sensibile și **NU TREBUIE NICIODATĂ VERSIONAT (comitat în Git)**.

4.  **Configurează și pornește baza de date locală (folosind Docker):**
    Proiectul utilizează Docker Compose pentru a gestiona serviciile locale necesare, inclusiv baza de date MySQL și serviciul Redis.

    ```bash
    docker-compose up -d
    ```
    Această comandă va construi și porni containerele definite în `docker/docker-compose.yaml` în fundal. Fișierele de inițializare SQL (ex: `docker/mysql/init-scripts/create-databases.sql`) sunt rulate automat de serviciul MySQL din Docker la prima pornire a containerului, creând schemele și utilizatorii necesari.

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
* `utils/`: Acest director conține fișiere utilitare (helper functions) și module de suport. Aici se găsesc funcții generice, clientul pentru baza de date (`dbClient.js`), sau module pentru generarea de date de test. Scopul este de a centraliza logica reutilizabilă care nu este direct legată de pașii specifici de test sau de Page Objects.
* `playwright.config.js`: Fișierul principal de configurare pentru Playwright. Aici sunt definite setările globale pentru rularea testelor (ex: browserele țintă, timeout-uri, rapoarte, fișiere de setup global).
* `.env`: Fișierul pentru variabilele de mediu. **Acest fișier nu trebuie niciodată versionat (comitat în Git) din motive de securitate.**
* `docker/`: Conține fișierele Docker (ex: `docker-compose.yaml`, `Dockerfile` pentru servicii precum `www.bpay.md` și `xapi.bpay.md`) necesare pentru a rula și configura serviciile locale.

---

Acum ai un `README.md` foarte detaliat! Copiază-l în fișierul tău `README.md` din rădăcina proiectului.
