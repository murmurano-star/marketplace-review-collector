# Marketplace Review Collector

Приложение для автоматического сбора отзывов с маркеплейсов **Wildberries** и **Ozon** по расписанию.

## Функциональность

✅ Сбор отзывов с Wildberries и Ozon  
✅ Фильтрация по диапазону дат  
✅ Экспорт в CSV с полной информацией  
✅ Скачивание фото из отзывов  
✅ Планировщик для автоматического сбора данных  
✅ Логирование всех операций  

## Требования

- Java 11 или выше
- Maven 3.6+
- Интернет-соединение

## Структура проекта

```
marketplace-review-collector/
├── src/main/java/com/marketplace/
│   ├── ReviewCollectorApplication.java    # Точка входа
│   ├── models/
│   │   ├── Review.java                    # Модель отзыва
│   │   ├── ReviewSource.java              # Тип источника
│   │   └── CollectionTask.java            # Задача сбора
│   ├── collectors/
│   │   ├── ReviewCollector.java           # Интерфейс сборщика
│   │   ├── WildberriesCollector.java      # Сборщик Wildberries
│   │   └── OzonCollector.java             # Сборщик Ozon
│   ├── export/
│   │   ├── CsvExporter.java               # Экспорт в CSV
│   │   └── PhotoDownloader.java           # Скачивание фото
│   ├── scheduler/
│   │   ├── ReviewCollectionJob.java       # Задача планировщика
│   │   └── SchedulerManager.java          # Управление планировщиком
│   └── config/
│       └── Config.java                    # Конфигурация
├── src/main/resources/
│   ├── logback.xml                        # Конфиг логирования
│   ├── quartz.properties                  # Конфиг планировщика
│   └── tasks.json                         # Задачи сбора
├── pom.xml                                # Maven конфиг
└── README.md                              # Этот файл
```

## Установка и запуск

### 1. Клонирование репозитория

```bash
git clone https://github.com/murmurano-star/marketplace-review-collector.git
cd marketplace-review-collector
```

### 2. Сборка проекта

```bash
mvn clean install
```

или

```bash
mvn clean package
```

### 3. Конфигурация задач

Отредактируйте файл `src/main/resources/tasks.json`:

```json
{
  "tasks": [
    {
      "id": "task_1",
      "name": "Сбор отзывов Wildberries",
      "marketplace": "WILDBERRIES",
      "productUrl": "https://www.wildberries.ru/catalog/product_id",
      "startDate": "2026-05-01",
      "endDate": "2026-06-06",
      "outputFile": "reviews_wildberries.csv",
      "downloadPhotos": true,
      "photosDirectory": "photos/wildberries"
    },
    {
      "id": "task_2",
      "name": "Сбор отзывов Ozon",
      "marketplace": "OZON",
      "productUrl": "https://www.ozon.ru/product/product_id/",
      "startDate": "2026-05-01",
      "endDate": "2026-06-06",
      "outputFile": "reviews_ozon.csv",
      "downloadPhotos": true,
      "photosDirectory": "photos/ozon"
    }
  ],
  "schedule": {
    "enabled": true,
    "cronExpression": "0 0 2 * * ?",
    "timezone": "Europe/Moscow"
  }
}
```

**Описание параметров:**
- `marketplace`: `WILDBERRIES` или `OZON`
- `productUrl`: URL товара на маркеплейсе
- `startDate`, `endDate`: Диапазон дат в формате `YYYY-MM-DD`
- `downloadPhotos`: Скачивать ли фото (true/false)
- `cronExpression`: Формат Cron для расписания (по умолчанию 2:00 каждый день)

### 4. Запуск приложения

**Один раз (без расписания):**
```bash
java -jar target/review-collector-1.0.0.jar --run-once
```

**С расписанием (фоновое выполнение):**
```bash
java -jar target/review-collector-1.0.0.jar
```

## Использование

### Запуск конкретной задачи

```bash
java -jar target/review-collector-1.0.0.jar --task-id task_1
```

### Список доступных команд

```bash
java -jar target/review-collector-1.0.0.jar --help
```

## Формат CSV

Экспортированный CSV содержит следующие колонки:

| Колонка | Описание |
|---------|---------|
| `id` | Уникальный идентификатор отзыва |
| `marketplace` | Источник (WILDBERRIES/OZON) |
| `product_id` | ID товара |
| `author` | Имя автора отзыва |
| `rating` | Рейтинг (1-5) |
| `title` | Заголовок отзыва |
| `text` | Текст отзыва |
| `date` | Дата публикации |
| `photos` | Список URL фото (если есть) |
| `helpful_count` | Количество "полезно" |
| `collected_at` | Время сбора данных |

## Примеры

### Пример 1: Сбор отзывов Wildberries

```json
{
  "id": "wb_example",
  "marketplace": "WILDBERRIES",
  "productUrl": "https://www.wildberries.ru/catalog/123456789",
  "startDate": "2026-05-01",
  "endDate": "2026-06-06",
  "outputFile": "reviews_wb.csv",
  "downloadPhotos": true,
  "photosDirectory": "photos/wb"
}
```

### Пример 2: Сбор отзывов Ozon

```json
{
  "id": "oz_example",
  "marketplace": "OZON",
  "productUrl": "https://www.ozon.ru/product/12345678/",
  "startDate": "2026-04-01",
  "endDate": "2026-06-06",
  "outputFile": "reviews_ozon.csv",
  "downloadPhotos": false
}
```

## Расписание (Cron выражения)

Примеры расписаний:

- `0 0 2 * * ?` - Каждый день в 2:00
- `0 0 * * * ?` - Каждый час
- `0 0 0 ? * MON` - Каждый понедельник в 00:00
- `0 0 */6 * * ?` - Каждые 6 часов
- `0 0 2 ? * MON-FRI` - Каждый будний день в 2:00

## Логирование

Логи записываются в файлы:
- `logs/application.log` - Основной лог
- `logs/error.log` - Ошибки

Уровень логирования можно изменить в `src/main/resources/logback.xml`

## Обработка ошибок

Приложение автоматически:
- Повторяет запросы при ошибках сети (3 попытки)
- Логирует все ошибки для анализа
- Продолжает работу при ошибке одного отзыва
- Сохраняет собранные данные при прерывании

## Важно

⚠️ **Убедитесь, что:**
- Используется реальный User-Agent браузера
- Между запросами соблюдаются паузы (не более 1 запроса в секунду)
- Вы соблюдаете `robots.txt` маркеплейсов
- Данные используются только в законных целях

## Ограничения

- Maximal запросов в минуту ограничен для избежания блокировки
- Фото может быть недоступно для скачивания
- Некоторые отзывы могут быть удалены модератором

## Лицензия

MIT License

## Контакты

При вопросах создавайте Issues в репозитории.
