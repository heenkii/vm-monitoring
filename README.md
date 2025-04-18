# VM monotoring 

## Структура проекта

```
├── files
│   └── node_exporter.service
├── grafana
│   └── provisioning
│       ├── dashboards
│       │   ├── dashboards.yml
│       │   └── node_exporter_fill.json
│       └── datasources
│           └── datasources.yml
├── templates
│   └── prometheus.yml.j2
├── docker-compose.yml
├── playbook.yml
├── README.md
└── Vagrantfile
```

## Описание основных файлов

### Vagrantfile
```
Определяет параметры виртуальной машины:

- Базируется на `ubuntu/focal64` (Ubuntu 20.04)
- Настройка приватной сети IP 192.168.50.4 и проброс порта 3000 для доступа к Grafana
- Задает параметры виртуальной машины (2 CPU, 2048 МБ RAM)
- Выполняет провижининг через Ansible с использованием `ansible_local`
```

### playbook.yml
```
- Установка системных пакетов
- Установка Docker
- Настройка пользователя Docker
- Установка Docker Compose
- Установка Node Exporter
- Создание необходимых каталоги для конфигурационных файлов Prometheus и Grafana.
- Деплой конфигурации Prometheus с использованием шаблона prometheus.yml.j2
- Развёртывание Docker Compose файла
- Копирование файлов для автоматической настройки источников данных и дашбордов Grafana
- Запуск контейнеров docker
```
### docker-compose.yml
```
Описывает два сервиса:
    - Prometheus для собора метрик с Node Exporter
    - Grafana для визуализации метрик собранных prometheus (при запуске контейнеров автоматически загружаются настройки дашбордов и источников данных из папки provisioning)
```

### templates/prometheus.yml.j2
```
Шаблон конфигурации Prometheus. В шаблоне используется переменная `host_ip`, которая передаётся из Vagrantfile.
```

### grafana/provisioning
```
Файлы, необходимые для автоматической настройки Grafana:
    - dashboards – настройки и JSON-конфигурация дашборда
    - datasources – настройки источников данных
```

### files/node_exporter.service
```
Файл systemd, описывающий сервис для Node Exporter
```


# Запуск проекта
```bash
git clone https://github.com/yourusername/vm-monitoring.git
cd vm-monitoring
vagrant up
```

После запуска по адресу http://localhost:3000 становится доступна Grafana (дефолтная авторизация admin/admin) с настроеным дашбордом Node Exporter Full
