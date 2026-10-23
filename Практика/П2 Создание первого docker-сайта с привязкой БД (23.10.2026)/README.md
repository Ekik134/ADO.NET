## Что изучалось
На занятии были рассмотрены основы работы c Docker: как правильно оформлять проект для создания docker-контейнеров и запуска веб-приложений, подключение docker-файлов к БД для корректной совместной работы при помощи ADO.NET.

## Суть задания
1. Повторить структуру проекта, предоставленную преподавателем;
2. Самостоятельно реализовать методы внутри файла DbHelper.cs через NpgsqlConnection/NpgsqlCommand/NpgsqlDataReader для взаимодействия с БД через веб-приложение:

```
Initialize() — DDL + сидинг.
GetAllParts() — SELECT + NpgsqlDataReader, цикл while (reader.Read()).
GetPartById(int id) — SELECT ... WHERE id = @id, обработка IsDBNull(..) для description.
AddPart(Part part) — INSERT с параметрами.
DecreaseStock(...) — UPDATE ... WHERE stock >= @qty.
```

3. Поднять сайт с помощью Docker.