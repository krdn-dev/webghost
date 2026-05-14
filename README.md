# WebGhost – Traffic Imitator

**WebGhost** — это программа (утилита), которая создаёт многостраничный корпоративный сайт и имитирует вокруг него реалистичную посещаемость.

**Зачем это нужно:** чтобы ваш прокси-трафик было невозможно отличить от обычного корпоративного сайта. WebGhost подходит для маскировки любого прокси-сервера: NaiveProxy, Hysteria, Xray и других.

## ✨ Что делает WebGhost

- **Генерирует полноценный сайт** — блог, раздел «О компании», контакты, карта сайта, robots.txt, sitemap.xml и все необходимые ресурсы (CSS, JS, изображения, шрифты)
- **Имитирует реальных посетителей** — разные браузеры (Chrome, Firefox, Safari, Edge), паузы чтения, переходы по ссылкам, отправка форм, UTM-метки
- **Добавляет фоновый шум** — запросы от поисковых ботов, сканеров безопасности, попытки доступа к несуществующим страницам
- **Поддерживает взаимную имитацию** — два сервера могут генерировать трафик друг для друга
- **Обновляется одной командой** — `webghost update`

## Быстрый старт

### Установка одной командой
```bash
wget https://github.com/krdn-dev/webghost/releases/latest/download/webghost-linux-amd64 -O /usr/local/bin/webghost
chmod +x /usr/local/bin/webghost
webghost --domain=example.com install
```

## Пошаговая установка

### 1. Скачать готовый бинарник
```bash
wget https://github.com/krdn-dev/webghost/releases/latest/download/webghost-linux-amd64
```

### 2. Установить
```bash
sudo cp webghost-linux-amd64 /usr/local/bin/webghost
sudo chmod +x /usr/local/bin/webghost
```

### 3. Создать сайт и настроить автозапуск
```bash
webghost --domain=example.com install
```

## 📲 Основные команды

| Описание | Команда |
|-----------|----------------------|
| **Установка сайта и настройка systemd-таймера** | webghost --domain=example.com install |
| **Только создание структуры сайта** | webghost --domain=example.com setup-site |
| **Запуск имитации трафика вручную** | webghost --domain=example.com simulate |
| **Обновить WebGhost до последней версии** | webghost update |
| **Показать все команды и примеры** | webghost help |

## 📲 Примеры использования

### Полная установка (сайт + автозапуск)
```bash
webghost --domain=example.com install
```

### Только создание сайта
```bash
webghost --domain=example.com setup-site
```

### Ручной запуск имитации
```bash
webghost --domain=example.com simulate
```

### С логированием в файл
```bash
webghost --domain=example.com --log=/var/log/webghost.log simulate
```

### Взаимная имитация с другим сервером
```bash
webghost --domain=example.com --remote=other-server.com simulate
```

### Взаимная имитация с логированием
```bash
webghost --domain=example.com --remote=other-server.com --log=/var/log/webghost.log simulate
```

### Как убедиться, что всё работает
```bash
cat /var/log/webghost-activity.log
```

В логе вы увидите посещения страниц (HTTP 200), загрузку ресурсов, смену User-Agent и фоновый шум.

## ❓ Часто задаваемые вопросы
### Можно ли узнать подробности о сигнатурах, которые WebGhost использует для имитации трафика?
Эта информация намеренно не раскрывается. Чем меньше деталей о внутренних алгоритмах и шаблонах попадает в открытый доступ, тем сложнее системам глубокого анализа трафика (DPI) разработать контрмеры. Разработчики на «той стороне» тоже читают документацию, поэтому самые эффективные настройки остаются внутри кода.

### Как убедиться, что WebGhost работает и имитация трафика активна?
Вся активность записывается в лог /var/log/webghost-activity.log.
Чтобы просмотреть накопленные записи, выполните команду в терминале: cat /var/log/webghost-activity.log.

### В логе вы увидите: посещения страниц (HTTP 200), запросы к ресурсам (favicon.ico, style.css, картинки), смену User‑Agent и эмуляцию разных браузеров, фоновый шум (редкие запросы к несуществующим страницам) и многое другое.

### Обязательно ли на удалённом сервере должен стоять WebGhost?
Технически подойдёт любой сервер, отвечающий на HTTPS-запросы. Но для полной реалистичности рекомендуется установить WebGhost на оба сервера.

### Нужен ли Go для работы WebGhost?
Нет, WebGhost поставляется как готовый скомпилированный бинарник. Go требуется только если вы хотите собрать его из исходников.

### Нужно ли устанавливать веб-сервер отдельно?
Да. WebGhost создаёт только структуру сайта и имитирует трафик. Веб-сервер (Caddy, Nginx и т.д.) должен быть установлен и настроен отдельно, чтобы сайт был доступен по HTTPS.

## 🛠️ Системные требования
Linux (amd64 или arm64)
Доступ к порту 443 (HTTPS)
Права root (для записи в /var/www/html и /usr/local/bin)
Установленный и настроенный веб-сервер

## 📄 Лицензия
Проект распространяется под лицензией https://img.shields.io/badge/License-MIT-blue.svg

Кратко: можно использовать, копировать, модифицировать, распространять с указанием авторства.

## 💰 Поддержать проект  
Если хотите поддержать развитие проекта, можете отправить небольшое пожертвование    
[![Bitcoin](https://img.shields.io/badge/Bitcoin-F7931A?style=flat&logo=bitcoin&logoColor=white)](https://www.blockchain.com/explorer/addresses/btc/bc1p4ttkpfrgzpm7nyymyzdgyd2y6z04s62nxpygk38yylcp3t47m98qwnuhen)
```bc1p4ttkpfrgzpm7nyymyzdgyd2y6z04s62nxpygk38yylcp3t47m98qwnuhen```

❤️ Спасибо за поддержку!
⭐ Поставьте звезду репозиторию, если скрипт вам помог!

