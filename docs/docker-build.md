# Metode pentru compilarea locală a imaginilor Docker

Acest proiect folosește acum funcția de compilare automată a `imaginilor Docker` de la `GitHub`. Dacă extrageți imaginea distribuită a proiectului și nu trebuie să o compilați singur, ignorați acest document.

Dacă ați modificat codul sursă și doriți să îl implementați și să îl rulați folosind `Docker`, puteți consulta următorii pași:

## 1、Pregătirea pentru mediu

Instalați Docker：
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## 2、Compilație Imagine

După ce ați modificat codul și trebuie să compilați o imagine nouă, trebuie să urmați acești pași.：

Pregătiți `numele de utilizator` și `noul număr de versiune`.

- Acest `nume de utilizator` este numele de utilizator înregistrat pe `docker hub`, de exemplu, `xiaozhi`. Desigur, dacă nu trebuie să trimiteți date către `docker hub`, îl puteți defini liber.
- Acest `număr de versiune nouă` este versiunea imaginii pe care ați compilat-o, cum ar fi `1.2.3`. Îl puteți personaliza după cum este necesar sau puteți utiliza un format de dată (cum ar fi `20260609`). Acest lucru este în principal pentru a-l distinge de numărul versiunii pe care o utilizați în prezent și, de asemenea, pentru a vă ajuta să vă amintiți când ați construit-o. Nu ar trebui să fie același cu numărul versiunii pe care o utilizați în prezent pe mașina dvs.

Navigați la directorul rădăcină al proiectului `xiaozhi-esp32-server` și compilați atât imaginile serverului, cât și cele web:

```bash
cd directorul rădăcină al proiectului

# Compilează imaginea serverului
docker build -f Dockerfile-server -t numele dvs. de utilizator/xiaozhi-esp32-server:număr nou de versiune .

# Compilează imaginea web
docker build -f Dockerfile-web -t Numele dvs. de utilizator/xiaozhi-esp32-server-web:Număr nou de versiune .

```

## 3、Modificați configurația docker-compose

```bash
cd main/xiaozhi-server
```

editati `docker-compose_all.yml` În fișier, înlocuiți versiunea imaginii cu versiunea pe care tocmai ați compilat-o：

```yaml
services:
  xiaozhi-esp32-server:
    image: Numele dvs. de utilizator/xiaozhi-esp32-server:Număr nou de versiune   # Schimbați adresa mirror
    ...

  xiaozhi-esp32-server-web:
    image: Numele dvs. de utilizator/xiaozhi-esp32-server-web:Număr nou de versiune   #Schimbați adresa mirror
    ...
```

## 4、重启服务

```bash
# 停止旧容器
docker compose -f docker-compose_all.yml down

# 启动新容器
docker compose -f docker-compose_all.yml up -d
```

## 5、验证

查看日志确认服务启动正常：

```bash
# 查看server日志
docker logs -f -n 50 xiaozhi-esp32-server

# 查看web日志
docker logs -f -n 50 xiaozhi-esp32-server-web
```
