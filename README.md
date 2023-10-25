

# Documentatie Facy

### Cuprins
1. Necesitati
2. Instalare
3. Instructiuni 
4. Descriere proiect

### Necesitati
- Node.js
- Python 3.10
- Docker
- Redis
- Rabbitmq
- Tauri
- Rust
- Python lib din necesitati.txt
- Linux || subsisteme de Linux pe Windows
### Instalare
- Clonam repozitoriul https://github.com/Al3x-Myku/Atestat


### Instructiuni
1. Pentru utilizarea aplicatiei trebuie sa avem pornite un Docker container cu Redis si unul cu RabbitMQ
2. Intr-un terminal navigam in folderul FacyFrontend si pornim aplicatia cu `npm run tauri dev`
3. Intr-un terminal navigam in folderul FacyBackend si executam codul din "Main-App" si "img-sentiment"
4. Dupa ce am uramt pasii anteriori putem sa folosim aplicatia.

### Descriere proiect
Facy este o aplicatie desktop care se foloseste de machine learning pentru a prelua date din poze cu fetele persoanelor. Pentru fiecare set de date se asigneaza un ID unic si ulterior sunt salvate in RedisDB. 
Pentru balansarea cererilor catre aplicatie folosim RabbitMQ ca message broker si trimitem requesturile catre workeri pentru a putea procesa mai multe cereri simultan.
Proiectul se folosete de Flask pentru a creea un server web pentru a comunica prin JSON intre aplicatie si workeri RabbitMQ.
Aplicatia ruleaza pe desktop folosinduse de frameworkurile: Vue.js, tauri si de Node.js. Folosind Tauri observam datele afisate de serverul web si le afisam pe aplicatie sub forma de procent.

### Functionalitate

# Procesare Imagini cu Fețe:

Facy utilizează algoritmi de învățare automată pentru a analiza și extrage informații din imagini care conțin fețe de persoane.

# Alocare de ID-uri Unice:

Pentru fiecare set de date prelucrat, se atribuie un ID. Acest ID este utilizat pentru a identifica și recupera datele în viitor.

# Stocare în RedisDB:

Datele prelucrate sunt salvate într-o bază de date Redis. Redis este o bază de date în memorie care este cunoscută pentru performanțele sale ridicate și capacitatea de a gestiona cantități mari de date în timp real.

# Sistem de Mesagerie cu RabbitMQ:

Pentru a echilibra încărcătura cererilor către aplicație, se folosește RabbitMQ ca un broker de mesaje. Acesta gestionează distribuția cererilor către workeri pentru a putea prelucra mai multe cereri simultan.

# Server Web cu Flask:

Pentru a facilita comunicarea între aplicație și workerii RabbitMQ, proiectul utilizează Flask, un framework de dezvoltare web pentru Python. Comunicarea se face prin intermediul formatului JSON.

# Interfață Utilizator cu Vue.js:

Interfața utilizatorului (UI) a aplicației este dezvoltată folosind Vue.js, un framework JavaScript pentru construirea interfețelor de utilizator reactive.

# Observarea Datelor cu Tauri:

Se pare că proiectul folosește Tauri pentru a observa și afișa datele furnizate de serverul web. Acesta este un instrument care facilitează integrarea aplicației desktop cu tehnologiile web.

# Utilizarea Node.js:

Proiectul utilizează Node.js, un mediu de execuție JavaScript pe partea serverului, pentru a gestiona operațiuni de rețea și pentru a facilita dezvoltarea aplicației.



### FAQs
1. 
Q: Poate fi folosit real-time pentru a arata emotiile?

A: La momentul actual aplicatia poate fi folosita doar cu imagini facute anterior.
2. 
Q: Pot acesa datele dupa ce am incarcat alta imagine?

A: Da, datele pot fi accesate dupa incarcarea altei imagini.
3. 
Q: Pozele sunt salvate?

A: Pozele nu sunt salvate, doar ID-ul asignat si datele extrase.



