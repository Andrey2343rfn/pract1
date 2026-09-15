# Проєкт IoT-системи: Розумна теплиця (200 м²)

## 1. Архітектурна діаграма системи

```mermaid
graph TD
    subgraph L4[Рівень 4: Застосунки]
        AppWeb[Веб-панель агронома]
        AppMobile[Мобільний застосунок]
        Alerts[Сповіщення Telegram]
    end

    subgraph L3[Рівень 3: Обробка даних]
        subgraph Cloud[Хмара]
            CloudDB[(База даних)]
            Analytics[Аналітика]
        end
        subgraph Fog[Локальний шлюз RPi 5]
            RuleEngine[Автоматика Node-RED]
            LocalDB[(Буфер SQLite)]
        end
    end

    subgraph L2[Рівень 2: Мережа]
        Net[RS-485 / Zigbee / MQTT]
    end

    subgraph L1[Рівень 1: Сприйняття та дія]
        subgraph Sensors[Датчики]
            S1[Температура та вологість]
            S2[Вологість грунту]
            S3[Освітленість]
            S4[Концентрація CO2]
        end
        subgraph Actuators[Виконавчі механізми]
            A1[Клапани поливу]
            A2[Вентиляція та кватирки]
            A3[LED-освітлення]
            A4[Опалення]
        end
    end

    Sensors --> Net
    Net --> Fog
    Fog --> Cloud
    Cloud --> L4
    RuleEngine --> Actuators
    Fog -.-> AppWeb
