# Tenderduty+HAQQ

Nodes HAQQ monitoring and notification system

Мониторинг работы ноды HAQQ

Контроль за нодой, в частности отслеживание таких важных метрик, как аптайм, высота сети, статус валидатора, подписанные и пропущенные блоки, мы настроим при помощи такого комплексного инструмента как Tenderduty.
![image](https://user-images.githubusercontent.com/87209795/194782743-1b5d092d-c63d-44f4-8056-7a80f4f8e7d4.png)
 
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
![image](https://user-images.githubusercontent.com/87209795/194782776-e5b99528-eeed-47a2-974c-e4b256454ddb.png)
![image](https://user-images.githubusercontent.com/87209795/194782788-c256fcc2-ca30-4759-bc43-f6c0523b7f95.png)

После настройки конфига запускаем:
docker run -d --name tenderduty -p "8888:8888" -p "28686:28686" --restart unless-stopped -v $(pwd)/config.yml:/var/lib/tenderduty/config.yml ghcr.io/blockpane/tenderduty:latest
# смотрим логи
docker logs -f --tail 20 tenderduty
![image](https://user-images.githubusercontent.com/87209795/194782807-00f6f8d3-47f4-4d1a-a440-14a682834cec.png)

Шаг 5. Отслеживаем в браузере

# узнаем адрес и вставляем в браузер echo -e "\033[0;32mhttp://$(wget -qO- eth0.me):8888/\033[0m" 
# http://100.100.100.100:8888/

Шаг 6. Настройка Telegram

Создаем своего бота и узнать ID своего telegram или ID необходимой нам группы в telegram. Для этого пишем боту @BotFather и вводим команды /start и /newbot - далее вводим имя бота - далее username (должно заканчиваться на bot). Бот выдаст token API, который надежно сохраняем и никому не показываем.
![image](https://user-images.githubusercontent.com/87209795/194782821-393992b4-e640-4d2b-b0e6-b489a869fb27.png)
![image](https://user-images.githubusercontent.com/87209795/194782833-6c2b0e4d-fd8f-437d-8fdd-58633ee56dde.png)

Узнаем свой ID или ID группы (для этого добавляем бота в группу). Для того, чтобы узнать свой ID боту @JsonViewBot отправляем любое сообщение
 ![image](https://user-images.githubusercontent.com/87209795/194782841-223fca46-bfdf-4d43-8d5f-d9ee7a0b7a5e.png)

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
