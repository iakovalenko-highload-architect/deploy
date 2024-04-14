# Репозиторий для локального запуска учебного проекта

## Инструкция по запуску:

1. Склонировать репозиторий
2. Инициализировать подмодули:
    ```sh
    git submodule init
    git submodule update --remote --merge
    ``` 
3. Запустить контейнеры:
    ```sh
    docker compose up -d
    ```
4. Публичное API будет доступно на http://localhost:8080

5. WebSocket сервис будет доступен по ws://localhost:1323/