# # Проєкт IoT-системи: Розумна теплиця (200 м²)

## 1. Архітектурна діаграма системи (Mermaid)

Архітектура побудована за класичною 4-рівневою моделлю IoT з виділеним рівнем туманних обчислень (Fog Computing) для локальної автономності.

```mermaid
flowchart TD
    %% Рівень 4: Застосунки
    subgraph L4["Рівень 4: Застосунки (Application Layer)"]
        direction TB
        AppWeb["Веб-дашборд агронома (Grafana / React)"]
        AppMobile["Мобільний застосунок керування (Flutter)"]
        AlertService["Сервіс нотифікацій (Telegram / SMS Gateway)"]
    end

    %% Рівень 3: Обробка даних
    subgraph L3["Рівень 3: Обробка даних (Processing / Cloud & Fog)"]
        direction TB
        subgraph CloudProcessing["Хмара (AWS / Cloud Broker)"]
            CloudDB[(База часових рядів InfluxDB / PostgreSQL)]
            Analytics["ML-аналітика врожайності та прогнозування"]
        end
        subgraph FogProcessing["Локальний шлюз (Fog Node - Raspberry Pi 5)"]
            RuleEngine["Локальний рушій правил (Node-RED)"]
            LocalDB[(Локальний буфер SQLite / Telegraf)]
            FailoverLogic["Модуль автономної логіки (Offline Fallback)"]
        end
    end

    %% Рівень 2: Мережа
    subgraph L2["Рівень 2: Мережа (Network Layer)"]
        direction TB
        NetProtocols["RS-485 (Modbus RTU) / Zigbee 3.0 / Wi-Fi"]
        TransportProtocols["MQTT через TLS (MQTTS) / HTTPS"]
    end

    %% Рівень 1: Сприйняття та дія
    subgraph L1["Рівень 1: Сприйняття та дія (Perception & Actuation)"]
        direction TB
        subgraph Sensors["Датчики"]
            S_TH["Датчики темп. та вологості повітря (DHT22 / SHT35)"]
            S_Soil["Ємнісні датчики вологості ґрунту (Capacitive Soil)"]
            S_Lux["Датчики освітленості (BH1750)"]
            S_CO2["Оптичні датчики CO2 (MH-Z19B NDIR)"]
        end
        subgraph Actuators["Виконавчі механізми"]
            A_Valve["Електромагнітні клапани крапельного поливу"]
            A_Vent["Сервоприводи кватирок / витяжні вентилятори"]
            A_Light["Фітосвітильники LED з ШІМ-димуванням"]
            A_Heat["Твердотільні реле нагрівачів / контуру опалення"]
        end
    end

    %% Зв'язки між рівнями
    Sensors -->|Modbus RTU / Zigbee| L2
    L2 -->|Збір даних| FogProcessing
    RuleEngine -->|Команди керування через L2| Actuators
    FogProcessing -->|MQTT over TLS (Інтернет)| CloudProcessing
    CloudProcessing --> L4
    FogProcessing -.->|Локальний Wi-Fi / fallback| AppWeb
