```mermaid
flowchart TD
    %% Рівень 4: Застосунки
    subgraph L4["Рівень 4: Застосунки (Application Layer)"]
        direction TB
        AppWeb["Веб-дашборд агронома (Grafana / React)"]
        AppMobile["Мобільний застосунок керування (Flutter)"]
        AlertService["Сервіс нотифікацій (Telegram / SMS)"]
    end

    %% Рівень 3: Обробка даних
    subgraph L3["Рівень 3: Обробка даних (Processing / Cloud & Fog)"]
        direction TB
        subgraph CloudProcessing["Хмара (AWS / Cloud Broker)"]
            CloudDB[("База часових рядів (InfluxDB)")]
            Analytics["ML-аналітика врожайності"]
        end
        subgraph FogProcessing["Локальний шлюз (Raspberry Pi 5)"]
            RuleEngine["Локальний рушій правил (Node-RED)"]
            LocalDB[("Локальний буфер (SQLite)")]
            FailoverLogic["Модуль автономної логіки"]
        end
    end

    %% Рівень 2: Мережа
    subgraph L2["Рівень 2: Мережа (Network Layer)"]
        direction TB
        NetProtocols["RS-485 / Zigbee 3.0 / Wi-Fi"]
        TransportProtocols["MQTT через TLS / HTTPS"]
    end

    %% Рівень 1: Сприйняття та дія
    subgraph L1["Рівень 1: Сприйняття та дія (Perception & Actuation)"]
        direction TB
        subgraph Sensors["Датчики"]
            S_TH["Датчики темп. та вологості (SHT35)"]
            S_Soil["Ємнісні датчики вологості ґрунту"]
            S_Lux["Датчики освітленості (BH1750)"]
            S_CO2["Оптичні датчики CO2 (MH-Z19B)"]
        end
        subgraph Actuators["Виконавчі механізми"]
            A_Valve["Електромагнітні клапани поливу"]
            A_Vent["Сервоприводи кватирок / вентилятори"]
            A_Light["LED-фітосвітильники"]
            A_Heat["Реле контуру опалення"]
        end
    end

    %% Зв'язки між рівнями
    Sensors --> L2
    L2 --> FogProcessing
    RuleEngine --> Actuators
    FogProcessing --> CloudProcessing
    CloudProcessing --> L4
    FogProcessing -.-> AppWeb
