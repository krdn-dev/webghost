# WebGhost – Traffic Imitator

**WebGhost** is a utility that generates a multi-page corporate website and simulates realistic traffic around it.

**Why is this needed:** To make it impossible to distinguish your HTTPS proxy traffic from regular corporate website traffic. WebGhost is suitable for masking proxy servers operating over HTTPS on port 443: NaiveProxy, Hysteria (with HTTP masking), Xray (VLESS+XTLS), Trojan, and others.

![WebGhost Site](https://github.com/krdn-dev/webghost/blob/main/site.PNG)

## ✨ What WebGhost Does

- **Generates a full-featured website** — blog, "About Us" section, contacts, sitemap, robots.txt, sitemap.xml, and all necessary resources (CSS, JS, images, fonts)
- **Simulates real visitors** — different browsers (Chrome, Firefox, Safari, Edge), reading pauses, link clicks, form submissions, UTM parameters
- **Adds background noise** — requests from search engine bots, security scanners, attempts to access non-existent pages
- **Supports mutual simulation** — two servers can generate traffic for each other
- **Updates with a single command** — `webghost update`

### 🎨 Unique Design for Each Domain

WebGhost does not create identical sites. During generation, the domain name is used as a source of entropy, so each domain receives:

- **Unique logo** — the company name in uppercase
- **Individual color scheme** — shades of accent color, gradients and transparencies vary slightly from site to site
- **Personalized contacts** — email and phone number are tied to your domain

Visually, all sites look consistent with a corporate style, but re-installing on a different domain will create a *similar but not identical* design. This makes it harder to create DPI signatures based on the site's appearance.

## 🚀 Quick Installation

```bash
wget https://github.com/krdn-dev/webghost/releases/latest/download/webghost-linux-amd64 -O /usr/local/bin/webghost
chmod +x /usr/local/bin/webghost
webghost --domain=example.com install
```
or
```bash
wget -O /usr/local/bin/webghost https://github.com/krdn-dev/webghost/releases/latest/download/webghost-linux-amd64 && chmod +x /usr/local/bin/webghost && webghost --domain=example.com install
```

## Step-by-Step Installation

### 1. Download the pre-built binary
```bash
wget https://github.com/krdn-dev/webghost/releases/latest/download/webghost-linux-amd64
```

### 2. Install it
```bash
sudo cp webghost-linux-amd64 /usr/local/bin/webghost
sudo chmod +x /usr/local/bin/webghost
```

### 3. Create the website and set up auto-start
```bash
webghost --domain=example.com install
```

## 📲 Main Commands

| Description | Command |
|-----------|----------------------|
| **Install website and configure systemd timer** | webghost --domain=example.com install |
| **Generate site structure only** | webghost --domain=example.com setup-site |
| **Manually run traffic simulation** | webghost --domain=example.com simulate |
| **Update WebGhost to the latest version** | webghost update |
| **Show all commands and examples** | webghost help или webghost |

## 📲 Usage Examples

### Full installation (site + auto-start)
```bash
webghost --domain=example.com install
```

### Generate site only
```bash
webghost --domain=example.com setup-site
```

### Run simulation
```bash
webghost --domain=example.com simulate
```

### Run simulation with file logging
```bash
webghost --domain=example.com --log=/var/log/webghost.log simulate
```

### Mutual simulation with another server
```bash
webghost --domain=example.com --remote=other-server.com simulate
```

### Mutual simulation with logging
```bash
webghost --domain=example.com --remote=other-server.com --log=/var/log/webghost.log simulate
```

### Verify everything is working
```bash
cat /var/log/webghost-activity.log
```
In the log, you will see page visits (HTTP 200), resource loading, User-Agent changes, and background noise.

## ❓ Часто задаваемые вопросы
### Можно ли узнать подробности о сигнатурах, которые WebGhost использует для имитации трафика?
Эта информация намеренно не раскрывается. Чем меньше деталей о внутренних алгоритмах и шаблонах попадает в открытый доступ, тем сложнее разработчикам систем глубокого анализа трафика (DPI) разработать контрмеры. Разработчики на «той стороне» тоже читают документацию, поэтому самые эффективные настройки остаются внутри кода.

### А что, DPI не видит, что трафик идёт с локального сервера, а не от реальных посетителей из интернета?
DPI провайдера видит общий поток данных, проходящий через сервер.
Он анализирует метаданные (IP, объём, тайминги), но не всегда может
определить точный источник запроса внутри сети. WebGhost смешивает
имитированный трафик с реальным, усложняя выделение настоящих
прокси-соединений на общем фоне. Однако некоторые продвинутые DPI
могут анализировать задержки или сетевые потоки (NetFlow) и заметить,
что часть запросов исходит с локального хоста. Именно поэтому мы
рекомендуем использовать WebGhost в режиме взаимной имитации — когда
два сервера обмениваются трафиком, создавая двустороннюю картину,
которую значительно сложнее отличить от реальной.

### Как распределяется имитация трафика в разных режимах?
WebGhost поддерживает два режима работы:

- **Локальный режим** — имитация посетителей только на вашем сервере.
  Используется по умолчанию, создаёт полноценную картину активного сайта.

- **Режим взаимной имитации** (`--remote`) — оба сервера обмениваются
  трафиком. Ваш сервер создаёт облегчённую имитацию для себя и полноценную
  для удалённого. Это делает трафик двусторонним и ещё более естественным
  для DPI.

Точные параметры (количество сессий, вероятность всплесков) не раскрываются
по соображениям безопасности — они являются частью стратегии обхода DPI.

### Как убедиться, что WebGhost работает и имитация трафика активна?
Вся активность записывается в лог `/var/log/webghost-activity.log`.
Чтобы просмотреть накопленные записи, выполните команду в терминале: `cat /var/log/webghost-activity.log`.

В логе вы увидите: посещения страниц (HTTP 200), запросы к ресурсам (favicon.ico, style.css, картинки), смену User‑Agent и эмуляцию разных браузеров, фоновый шум (редкие запросы к несуществующим страницам) и многое другое.

### Обязательно ли на удалённом сервере должен стоять WebGhost?
Для максимальной реалистичности — да. WebGhost при взаимной имитации
запрашивает те же страницы, которые генерирует при setup-site. Если
на удалённом сервере нет такого сайта, он будет отвечать ошибками 404,
что неестественно для корпоративного портала и может привлечь внимание DPI.

### Нужен ли веб‑сервер для работы WebGhost?
Да, веб‑сервер обязателен. WebGhost создаёт сайт и имитирует трафик, но сам не обрабатывает входящие HTTPS‑запросы. Веб‑сервер (Caddy, Nginx и т.д.) должен быть установлен и настроен отдельно, чтобы сайт был доступен из интернета. Если вы используете [NaiveProxy Manager](https://github.com/krdn-dev/naiveproxy-manager), веб‑сервер устанавливается автоматически.

## 🛠️ Системные требования
- Linux (amd64 или arm64)
- Домен, привязанный к серверу (A-запись должна указывать на ваш IP)
- Доступ к порту 443 (HTTPS)
- Права root (для записи в /var/www/html и /usr/local/bin)
- Работающий веб‑сервер, отвечающий по HTTPS на вашем домене (Caddy, Nginx и т.д.)

## 📄 Лицензия
Проект распространяется под проприетарной лицензией  [![License](https://img.shields.io/badge/License-Proprietary-red.svg)]()    
Все права защищены.

Разрешено:
- Свободное использование бинарника в личных и коммерческих целях
- Копирование и распространение бинарника с сохранением авторства

Запрещено:
- Модификация, декомпиляция и reverse engineering
- Публикация исходного кода или его производных

## 💰 Поддержать проект  
Если хотите поддержать развитие проекта, можете отправить небольшое пожертвование   

[![Bitcoin](https://img.shields.io/badge/Bitcoin-F7931A?style=flat&logo=bitcoin&logoColor=white)](https://www.blockchain.com/explorer/addresses/btc/bc1p4ttkpfrgzpm7nyymyzdgyd2y6z04s62nxpygk38yylcp3t47m98qwnuhen)
```bc1p4ttkpfrgzpm7nyymyzdgyd2y6z04s62nxpygk38yylcp3t47m98qwnuhen```

❤️ Спасибо за поддержку!    
⭐ Поставьте звезду репозиторию, если скрипт вам помог!

