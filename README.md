# Grafana-Prometheus-VictoriaMetrics
## Описание
<br>ПО: VirtualBox, Docker + Docker Compose, Grafana, Prometheus, NodeExporter; ОС: OracleLinux.
<br>Задача:
<br>![380424642-6ccebb43-249b-4d0a-b1e7-5bb8dab805e1](https://github.com/user-attachments/assets/5e4c8f1d-8e96-485a-b57b-6ff2a452693a)
Далее переходим к установке docker с использованием grafana, вводим следующий набор команд:

1. sudo yum install wget
   
• устанавливает утилиту wget на вашу систему
![image](https://github.com/user-attachments/assets/5aac8278-8def-41e5-9e5c-df23108ec75b)

2. sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo

• Скачиваем файл репозитория

3. sudo yum install docker-ce docker-ce-cli containerd.io
   
• Устанавливаем docker

4. sudo systemctl enable docker --now
   
• Запускаем его и разрешаем автозапуск

5. sudo yum install curl

• Для этого сначала убедимся в наличие пакета curl

6. COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)

• Объявление переменной COMVER, полученной в результате curl запроса, хранящей в себе номер последней версии Docker Compose

7. sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose

• Теперь скачиваем скрипт docker-compose последней версии, используя объявленную ранее переменную и помещаем его в каталог /usr/bin

8. sudo chmod +x /usr/bin/docker-compose

• Предоставление прав на выполнение файла docker-compose.

9.docker-compose --version

• Проверка установленной версии Docker Compose.

![image](https://github.com/user-attachments/assets/82152d4e-d1df-4331-8768-ab54e2cb1628)

• Можно скачать git прямо из командной строки прописав Y

10. git clone https://github.com/skl256/grafana_stack_for_docker.git
    
    • выдаст ошибку и предложит скачать git, согласиться и продолжить

11. cd grafana_stack_for_docker
    
• переход в папку

12. sudo mkdir -p /mnt/common_volume/swarm/grafana/config

• команда создаёт полный путь /mnt/common_volume/swarm/grafana/config, включая все необходимые промежуточные каталоги, если они ещё не существуют.

13. sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data}

• команда создаёт структуру каталогов для Grafana и связанных с ней компонентов, если они ещё не существуют.

14. sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}

• все файлы и каталоги в указанных директориях будут переданы в собственность текущему пользователю и его группе

15. touch /mnt/common_volume/grafana/grafana-config/grafana.ini

• файл grafana.ini уже существует, команда обновит его временные метки (время последнего доступа и изменения). Если файл не существует, команда создаст новый пустой файл с указанным именем по указанному пути.

16. cp config/* /mnt/common_volume/swarm/grafana/config/

• команда копирует все файлы и подкаталоги из директории config в директорию /mnt/common_volume/swarm/grafana/config/

17. mv grafana.yaml docker-compose.yaml
    
• команда переименовывает файл grafana.yaml в docker-compose.yaml. Ничего не покажет, но можно проверить при помощи команды ls

18. sudo docker compose up -d

• команда создает и запускает контейнеры в фоновом режиме, используя конфигурацию из файла docker-compose.yml, с правами суперпользователя.

![image](https://github.com/user-attachments/assets/5d2c0e4f-ac1b-4259-a210-82daff24fb61)

19. sudo vi docker-compose.yaml

•команда открывает файл docker-compose.yaml в текстовом редакторе vi с правами суперпользователя, что позволяет вам редактировать его содержимое.

• Нас перекинет в текстовый редактор

• Что-бы что-то изменить в тесковом редакторе нажмите insert на клавиатуре

• Что бы сохранить что-то в этом документе нажимаем Esc пишем :wq! В этом текставом редакторе мы должны поставить node-exporter после services

![image](https://github.com/user-attachments/assets/23f8604a-e412-4ec1-86fc-76a31b7fd516)

20. sudo vi prometheus.yaml 
• команда открывает файл prometheus.yaml в текстовом редакторе vi с правами суперпользователя.

• /mnt/common_volume/swarm/grafana/config/prometheus.yaml - исправить targets: на exporter:9100,

![image](https://github.com/user-attachments/assets/c4081a99-10bf-4082-bdb6-926925ae6bc6)

# Grafana

* переходим на сайт `localhost:3000`
    * User & Password GRAFANA: `admin`
    * Код графаны: `3000`
    * Код прометеуса: `http://prometheus:9090`
* в меню выбираем вкладку Dashboards и создаем Dashboard
    * ждем кнопку +Add visualization, а после "Configure a new data source"
    * выбираем Prometheus
    * Connection
    * `http://prometheus:9090`
* Authentication
    * Basic authentication
        * User: `admin`
        * Password: `admin`
        * Нажимаем на Save & test и должно показывать зелёную галочку
* в меню выбираем вкладку Dashboards и создаем Dashboard
    * ждем кнопку "Import dashboard"
    * Find and import dashboards for common applications at grafana.com/dashboards: 1860 //ждем кнопку Load
    * Select Prometheus ждем кнопку "Import"

  ![image](https://github.com/user-attachments/assets/b814fd91-584f-48af-b5c7-5d5ee70f01ec)

  ![image](https://github.com/user-attachments/assets/710d6aa5-a78c-430f-b099-30aa86810159)

![image](https://github.com/user-attachments/assets/4753540f-88ce-4b73-a17e-27a8a8648d29)

![image](https://github.com/user-attachments/assets/9eb93b7e-6f47-4e50-bdb6-e5a411840592)

![image](https://github.com/user-attachments/assets/b28c780c-bb79-42b5-9ca1-f2b475c03b86)

# VictoriaMetrics

Для начала изменим docker-compose.yaml

1. `cd grafana_stack_for_docker`

• команда cd grafana_stack_for_docker изменяет текущий рабочий каталог на каталог grafana_stack_for_docker.

2. `sudo vi docker-compose.yaml`

• команда sudo открывает файл docker-compose.yaml в редакторе vi с правами суперпользователя.

![image](https://github.com/user-attachments/assets/1a957374-26c9-4b6f-96c1-0741d8d1b745)

В самом текстовом редакторе после prometheus вставляем

![image](https://github.com/user-attachments/assets/b25ebd84-0173-4e2c-9fe5-c94b7c290a37)

Захом в connection
там где мы писали http//:prometheus:9090 пишем http:victoriametrics:9090 И заменяем имя из "Prometheus-2" в "Vika"
нажимаем на dashboards add visualition выбираем "Vika"
снизу меняем на "code"
Переходим в терминал и пишем

3. `echo -e "# TYPE OILCOINT_metric1 gauge\nOILCOINT_metric1 0" | curl --data-binary @- http://localhost:8428/api/v1/import/prometheus  `

• команда отправляет бинарные данные (например, метрики в формате Prometheus) на локальный сервер, который слушает на порту 8428.

4. `curl -G 'http://localhost:8428/api/v1/query' --data-urlencode 'query=OILCOINT_metric1'`

• команда делает запрос к API для получения данных по метрике OILCOINT_metric1

• команда выводит текст, который может быть использован для определения метрики в формате, совместимом с Prometheus

• команда выводит информацию о типе и значении этой метрики в формате, который может быть использован системой мониторинга Prometheus.

Значение 0 меняем на любое другое

Копируем переменную OILCOINT_metric1 и вставляем в query

Нажимаем run

![image](https://github.com/user-attachments/assets/39f47940-170d-40f0-ad42-a3044e2a6e7a)

![image](https://github.com/user-attachments/assets/6a32be9a-b718-487f-8bd8-ae83c4d59b29)

Копируем переменную OILCOINT_metric1 и вставляем в code

![image](https://github.com/user-attachments/assets/9594d407-0d9a-42bb-96ff-f3942c624620)














