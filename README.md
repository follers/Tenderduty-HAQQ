# Tenderduty+HAQQ

Nodes HAQQ monitoring and notification system

#Мониторинг работы ноды HAQQ

Контроль за нодой, в частности отслеживание таких важных метрик, как аптайм, высота сети, статус валидатора, подписанные и пропущенные блоки, мы настроим при помощи такого комплексного инструмента как Tenderduty.
 
Для установки нам понадобится отдельный сервер, например VPS, можно с уже установленной нодой.
Шаг 1. Подготовка сервера
# обновляем репозитории
sudo apt update && sudo apt upgrade -y

# устанавливаем необходимые утилиты
sudo apt install curl build-essential git wget jq make gcc tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y

Шаг 2. Установка Docker

apt update && \ apt install apt-transport-https ca-certificates curl software-properties-common -y && \ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && \ add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && \ apt update && \ apt-cache policy docker-ce && \ sudo apt install docker-ce -y && \ docker –version

Шаг 3. Устанавливаем Tenderduty

mkdir tenderduty && cd tenderduty docker run --rm ghcr.io/blockpane/tenderduty:latest -example-config >config.yml

Шаг 4. Скачиваем и редактируем конфиг файл

wget -O $HOME/tenderduty/config.yml 
https://github.com/follers/Tenderduty-HAQQ/blob/92e5b231df4c5f0f2a04d54606606b36313d5963/config.yml

Пример конфиг файла

Базовые настройки:
имя сети:
chain-id:
valoper_address:
url:

 


 

 После настройки конфига запускаем:
docker run -d --name tenderduty -p "8888:8888" -p "28686:28686" --restart unless-stopped -v $(pwd)/config.yml:/var/lib/tenderduty/config.yml ghcr.io/blockpane/tenderduty:latest
# смотрим логи
docker logs -f --tail 20 tenderduty

 

Шаг 5. Отслеживаем в браузере

# узнаем адрес и вставляем в браузер echo -e "\033[0;32mhttp://$(wget -qO- eth0.me):8888/\033[0m" 
# http://100.100.100.100:8888/

Шаг 6. Настройка Telegram

Создаем своего бота и узнать ID своего telegram или ID необходимой нам группы в telegram. Для этого пишем боту @BotFather и вводим команды /start и /newbot - далее вводим имя бота - далее username (должно заканчиваться на bot). Бот выдаст token API, который надежно сохраняем и никому не показываем.

 
 

Узнаем свой ID или ID группы (для этого добавляем бота в группу). Для того, чтобы узнать свой ID боту @JsonViewBot отправляем любое сообщение
 

добавляем ID и token API в наш конфиг файл config.yml, после чего перезагружаем Tenderduty.

Полезные команды:

# посмотреть установленные образы
docker images
# посмотрнть запущенные контейнеры
docker ps
# остановить контейнер
docker stop tenderduty
# перезапустить контейнер
docker restart tenderduty
# посмотреть логи
docker logs -f --tail 20 tenderduty







