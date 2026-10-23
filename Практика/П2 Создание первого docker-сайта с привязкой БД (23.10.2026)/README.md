## Что изучалось
На занятии были рассмотрены основы работы c Docker: как правильно оформлять проект для создания docker-контейнеров и запуска веб-приложений, подключение docker-файлов к БД для корректной совместной работы при помощи ADO.NET.

## Структура папки

| Элемент | Краткое описание |
| :--- | :--- |
| **SCRINHOTS** | Каталог со скриншотами практики.* |
| **README.md** | Файл с описанием темы занятия и сутью задания. |
| **практика.md** | Файл с выполненной практикой. |
| **autoparts-docker.rar** | Заархивированная папка с исходным кодом проекта. |

## Суть задания
1. Повторить структуру проекта, предоставленную преподавателем (https://github.com/yaroslavrychikin-crypto/ado.net/blob/main/practica1.md);
2. Самостоятельно реализовать методы внутри файла DbHelper.cs через NpgsqlConnection/NpgsqlCommand/NpgsqlDataReader для взаимодействия с БД через веб-приложение:

```
Initialize() — DDL + сидинг.
GetAllParts() — SELECT + NpgsqlDataReader, цикл while (reader.Read()).
GetPartById(int id) — SELECT ... WHERE id = @id, обработка IsDBNull(..) для description.
AddPart(Part part) — INSERT с параметрами.
DecreaseStock(...) — UPDATE ... WHERE stock >= @qty.
```

3. Поднять сайт с помощью Docker.