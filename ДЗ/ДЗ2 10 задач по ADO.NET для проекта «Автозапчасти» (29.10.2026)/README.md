# Что изучалось

В ходе выполнения задания были рассмотрены практические приёмы работы с ADO.NET и Npgsql при разработке ASP.NET Core MVC-приложения «Автозапчасти», поднятого с помощью Docker и работающего с СУБД PostgreSQL. Были изучены параметризованные запросы и защита от SQL-инъекций, работа с `NpgsqlCommand` и `NpgsqlDataReader`, транзакции, обработка исключений, асинхронные операции, а также принципы работы пула соединений Npgsql и настройка `Maximum Pool Size`.

## Структура папки

| Элемент | Краткое описание |
| :--- | :--- |
| **SCRINSHOTS** | Каталог со скриншотами практики. |
| **README.md** | Файл с описанием темы занятия и сутью задания. |
| **задание.md** | Файл с выполненным заданием. |
| **autoparts-docker.rar** | Заархивированная папка с исходным кодом проекта. |

## Суть задания

Разработать и запустить в Docker веб-приложение «Автозапчасти» на ASP.NET Core MVC с доступом к PostgreSQL через Npgsql, реализовав десять задач по ADO.NET:

1. **Поиск запчастей по названию и артикулу** — метод `SearchParts(string query)` в `DbHelper` с параметризованным `ILIKE` (защита от SQL-инъекций); action `Search` в контроллере и строка поиска над таблицей во view.
2. **Удаление запчасти с проверкой результата** — `DeletePart(int id)` возвращает `true` только если `ExecuteNonQuery()` вернул больше нуля; `POST`-action с антифорджери, кнопка «Удалить» с `confirm()`, сообщение через `TempData`.
3. **Статистика по складу одним запросом** — `GetStats()` возвращает кортеж `(int totalParts, decimal totalValue, decimal avgPrice)`; SQL с `COUNT`, `SUM`, `AVG` и `COALESCE`; чтение через `NpgsqlDataReader`, вывод трёх карточек на `/Parts/Stats`.
4. **Оформление заказа в транзакции** — таблица `orders`, метод `PlaceOrder(int partId, int quantity)`: чтение `price`/`stock`, проверка остатка, `INSERT` в `orders`, `UPDATE` остатка, `Commit`; при нехватке — `Rollback`; все команды получают объект `tx`.
5. **Массовое поступление товара с откатом при ошибке** — `Restock(List<(int PartId, int AddQty)> items)` в одной транзакции; если хотя бы один `PartId` не найден (`ExecuteNonQuery() == 0`) — откат всей транзакции.
6. **«Сырое» чтение через `NpgsqlDataReader`** — `GetRawParts()` вручную раскладывает строки в `Dictionary<string, object?>` с использованием `FieldCount`, `GetName(i)`, `GetValue(i)`, `IsDBNull(i)`; модель `Part` не используется.
7. **`DataTable` через `NpgsqlDataAdapter`** — `GetPartsDataTable()` заполняет `DataTable` через `adapter.Fill(dt)`; `GetPartsDataSet()` с двумя таблицами (`parts` и `orders`) в одном `DataSet` — демонстрация отсоединённого режима.
8. **Асинхронная версия `GetAllParts`** — `GetAllPartsAsync()` с `OpenAsync`, `ExecuteReaderAsync`, `ReadAsync`, `await using`; action `Index` становится `async Task<IActionResult>`; без `.Result` и `.Wait()`.
9. **Обработка уникального ограничения (`SqlState`)** — ограничение `uq_parts_article UNIQUE (article)`; `TryAddPart(Part part)` возвращает `(bool Ok, string? Error)`; при `PostgresException` с `SqlState == "23505"` возвращается понятная ошибка через `ModelState.AddModelError`.
10. **Пул соединений и `Maximum Pool Size`** — в строке подключения `Maximum Pool Size=5;Minimum Pool Size=1`; endpoint `/Parts/PoolTest`, одновременно (`Task.WhenAll`) открывающий 20 соединений с `SELECT 1`; проверка в логах Postgres, что реальных соединений не больше 5; объяснение, почему `using` вокруг `NpgsqlConnection` возвращает соединение в пул, а не закрывает физическое подключение.