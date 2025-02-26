# Bezrukavaya-K-ISP-21-LAB-1


Для начала я установила Linux Oracle на VirtualBox, выделила 2 ядра и 4096 МБ оперативной памяти. При установке операционной системы выбрала английский язык (eng).

Кроме того, прямо перед началом основной работы я установила гостевые дополнения (Guest Additions) для активации общего буфера обмена, а также для возможности растягивания окна.

Дополнения гостевой системы (Guest Additions) представляют собой специальные утилиты и драйверы, которые устанавливаются в гостевую ОС при использовании виртуализации. Они необходимы для улучшения взаимодействия между основной и гостевой системами.
  
                                                   Начало работы 22.02.2025
Сейчас установлены пакеты wget и curl с помощью пакетного менеджера yum.

![2025-02-26_20-32-54](https://github.com/user-attachments/assets/512416ec-86ed-4d05-843a-b9d63a24e33f)

    sudo yum install wget
    
wget — это командная утилита, предназначенная для загрузки файлов с веб-серверов.

curl — это инструмент командной строки, который позволяет передавать данные на сервер или с него, поддерживая различные протоколы, такие как HTTP, HTTPS, FTP и другие.

Процесс установки файла репозитория Docker CE для CentOS.

![2025-02-26_20-34-02](https://github.com/user-attachments/assets/33513ac6-774f-431c-9de4-6a7aa54349ea)

    sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo

-P /etc/yum.repos.d/ — определяет каталог, куда будет сохранён загруженный файл.

https://download.docker.com/linux/centos/docker-ce.repo — это URL-адрес файла репозитория Docker CE для CentOS.

В результате команда была успешно выполнена, и файл Docker CE был загружен и сохранён в указанной папке. Он нужен для настройки репозитория Docker, что позволит менеджеру пакетов yum устанавливать пакеты Docker CE в нашу систему.

Установка и настройка необходимых пакетов для работы с Docker.

![2025-02-26_20-46-29](https://github.com/user-attachments/assets/aabe3bf5-653a-438c-b0ba-b11be390ad21)

    sudo yum install docker-ce docker-ce-cli containerd.io

docker-ce — это основной пакет Docker Community Edition. docker-ce-cli — это интерфейс командной строки для управления Docker. containerd.io — это контейнерный рантайм, который использует Docker.

Команда была успешно выполнена, и все компоненты, необходимые для работы с Docker CE, были установлены в систему. Это позволяет использовать Docker для создания и управления контейнерами на данном устройстве.

Данный шаг необходим для того, чтобы Docker мог функционировать после завершения установки.

![2025-02-26_22-26-28](https://github.com/user-attachments/assets/669f8559-ec89-43c0-b471-5dfc7b28f02d)

    sudo systemctl enable docker --now
    
Создана символическая ссылка /etc/systemd/system/multi-user.target.wants/docker.service, ссылающаяся на /usr/lib/systemd/system/docker.service.

Теперь Docker готов к использованию и будет автоматически запускаться при каждом загрузке системы.

Создаем переменную COMVER, которая содержит номер последней версии Docker Compose, полученный с помощью запроса curl. Затем скачиваем скрипт docker-compose последней версии, используя эту переменную, и помещаем его в каталог /usr/bin.

![2025-02-26_20-51-17](https://github.com/user-attachments/assets/13b41b93-55cc-43c6-873f-dd4898fadb54)
![2025-02-26_20-52-31](https://github.com/user-attachments/assets/fad155ad-8991-4e6f-9776-bd88369e9c9d)

    COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)

.


    sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose


Предоставление прав на выполнение файла docker-compose.

![2025-02-26_20-54-22](https://github.com/user-attachments/assets/ccea6b0a-a0f3-440e-9b8e-a21b05f712f6)

    sudo chmod +x /usr/bin/docker-compose

Проверка установленной версии Docker Compose.

![2025-02-26_20-54-40](https://github.com/user-attachments/assets/5a801a34-5f63-46bf-b871-b29fa58bdb89)

    docker-compose --version

Когда мы вводим команду git clone https://github.com/skl256/grafana_stack_for_docker.git, на нашем компьютере создается папка с этим проектом, в которой окажутся все файлы, доступные на GitHub. Это дает возможность работать с этими файлами локально, например, вносить изменения или просто просматривать их.

![2025-02-26_20-55-54](https://github.com/user-attachments/assets/e171ca94-b5d6-41e3-a81c-4e1d5926fc1f)

    git clone https://github.com/skl256/grafana_stack_for_docker.git

Данной командой мы перешли в папку, которуй созадли ранее.

![2025-02-26_20-57-31](https://github.com/user-attachments/assets/942e5280-ca60-4de3-a180-740b7da94896)

    cd grafana_stack_for_docker


Эта команда создает нужную директорию, а также все её родительские директории.

![2025-02-26_20-58-27](https://github.com/user-attachments/assets/9292f539-4a27-4bb5-a282-1691eed3c357)

    sudo mkdir -p /mnt/common_volume/swarm/grafana/config

Создаем три директории: grafana-config, grafana-data, и prometheus-data внутри /mnt/common_volume/grafana/.

![2025-02-26_20-59-47](https://github.com/user-attachments/assets/5694b82c-e2f8-4dbb-a7de-28abdceb454f)

    sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data}
    
Меняем владельца и группу для указанных директорий, а также для всех их файлов и поддиректорий, установив текущего пользователя и его группу.

![2025-02-26_21-00-52](https://github.com/user-attachments/assets/304641e0-15ba-447f-9997-faa88cd99237)

    sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}

Данная команда переносит все файлы из папки config в другую папку /mnt/common_volume/swarm/grafana/config/.

![2025-02-26_21-01-51](https://github.com/user-attachments/assets/c2d0b7f2-f6c5-431a-8708-e5554039828e)

    cp config/* /mnt/common_volume/swarm/grafana/config/

Эта команда изменяет имя файла grafana.yaml на docker-compose.yaml. Это может оказаться полезным, если вы планируете использовать конфигурацию Grafana в качестве настройки для Docker Compose.

![2025-02-26_21-03-14](https://github.com/user-attachments/assets/a1443165-367a-485f-a853-2a53851bdf3d)

    mv grafana.yaml docker-compose.yaml 

С помощью этой команды мы запускаем все контейнеры, указанные в нашем файле docker-compose.yaml, в фоновом режиме с привилегиями суперпользователя. Это удобно для развертывания сложных приложений, состоящих из нескольких контейнеров, например, стека Grafana.
-d - Этот флаг означает "запустить в фоновом режиме".
pwd - Команда pwd в терминале показывает, в какой папке вы сейчас находитесь.

![2025-02-27_00-49-21](https://github.com/user-attachments/assets/c779a077-8be4-4b59-8beb-61dfb2f9e70d)
![2025-02-26_21-06-57](https://github.com/user-attachments/assets/c7fdde55-d9f5-4a55-a2c2-7ec9bcf84850)

Команда запускает контейнеры в фоновом режиме

    sudo docker compose up -d

![2025-02-26_21-15-19](https://github.com/user-attachments/assets/f5e1335e-b6a1-4cea-9c07-4c23d006b570)

Эта команда, в свою очередь, играет важную роль, так как она останавливает активные контейнеры, но не удаляет их. Контейнеры остаются в состоянии 'stopped' и могут быть перезапущены с помощью команды sudo docker-compose start. При этом данные, тома, сеть и настройки контейнеров сохраняются.
Данные, тома, сеть и конфигурации контейнеров сохраняются.

      sudo docker-compose stop
      
Благодаря другой команде 
    
    sudo docker-compose down
Удаляем контейнеры (docker ps -a*).

Так-же удаляет созданную сеть (docker network ls**).

Не может удалять тома по умолчанию (но можно добавить --volumes для полного удаления данных).
