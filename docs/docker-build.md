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
cd 项目根目录

# 编译server镜像
docker build -f Dockerfile-server -t 你的用户名/xiaozhi-esp32-server:新的版本号 .

# 编译web镜像
docker build -f Dockerfile-web -t 你的用户名/xiaozhi-esp32-server-web:新的版本号 .

```

## 3、修改docker-compose配置

```bash
cd main/xiaozhi-server
```

编辑 `docker-compose_all.yml` 文件，将镜像版本替换为你刚才编译的版本：

```yaml
services:
  xiaozhi-esp32-server:
    image: 你的用户名/xiaozhi-esp32-server:新的版本号   # 修改为你的镜像地址
    ...

  xiaozhi-esp32-server-web:
    image: 你的用户名/xiaozhi-esp32-server-web:新的版本号   #修改为你的镜像地址
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
