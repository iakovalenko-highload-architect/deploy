# Репозиторий для локального запуска учебного проекта

## Инструкция по запуску:

1. Склонировать репозиторий
2. Инициализировать подмодули:
```sh
git submodule init
git submodule update --remote --merge
``` 
3. Запустить контнейнеры с базой:
```sh
docker compose up master worker manager -d
```
4. Инициализировать БД скриптом [service-dialog/db/master/init.sql](https://github.com/iakovalenko-highload-architect/service-dialog/blob/hw-6/db/master/init.sql)
```sh
docker exec -it deploy_master psql -U postgres
```
```sql
CREATE TABLE dialogs (
    id UUID NOT NULL DEFAULT gen_random_uuid() PRIMARY KEY,
    user_id_1 UUID NOT NULL,
    user_id_2 UUID NOT NULL
);

CREATE TABLE messages (
    id UUID NOT NULL DEFAULT gen_random_uuid(),
    dialog_id UUID NOT NULL,
    from_id UUID NOT NULL,
    to_id UUID NOT NULL,
    text_ TEXT,
    created_at DATE NOT NULL DEFAULT NOW(),
    updated_at DATE NOT NULL DEFAULT NOW(),

   FOREIGN KEY (dialog_id) REFERENCES dialogs(id)
);

SELECT create_distributed_table('dialogs', 'id');
SELECT create_distributed_table('messages', 'dialog_id', colocate_with => 'dialogs');
```

5. Запустить оставшиеся контнейнеры:
```sh
docker compose up -d
```