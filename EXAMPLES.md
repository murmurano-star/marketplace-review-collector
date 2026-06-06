# Examples

## Пример 1: Сбор отзывов Wildberries

### Конфиг tasks.json

```json
{
  "tasks": [
    {
      "id": "wb_example",
      "name": "Wildberries - Пример товара",
      "marketplace": "WILDBERRIES",
      "productUrl": "https://www.wildberries.ru/catalog/215638903",
      "startDate": "2026-05-01",
      "endDate": "2026-06-06",
      "outputFile": "output/wildberries_reviews.csv",
      "downloadPhotos": true,
      "photosDirectory": "output/photos/wb"
    }
  ],
  "schedule": {
    "enabled": false,
    "cronExpression": "0 0 2 * * ?",
    "timezone": "Europe/Moscow"
  }
}
```

### Запуск

```bash
mvn clean package
java -jar target/review-collector-1.0.0.jar --run-once
```

### Результаты

- Файл: `output/wildberries_reviews.csv`
- Фото: `output/photos/wb/[uuid].jpg`

---

## Пример 2: Сбор отзывов Ozon

### Конфиг tasks.json

```json
{
  "tasks": [
    {
      "id": "oz_example",
      "name": "Ozon - Пример товара",
      "marketplace": "OZON",
      "productUrl": "https://www.ozon.ru/product/123456789/",
      "startDate": "2026-04-01",
      "endDate": "2026-06-06",
      "outputFile": "output/ozon_reviews.csv",
      "downloadPhotos": false,
      "photosDirectory": "output/photos/oz"
    }
  ],
  "schedule": {
    "enabled": false,
    "cronExpression": "0 0 2 * * ?",
    "timezone": "Europe/Moscow"
  }
}
```

### Запуск

```bash
java -jar target/review-collector-1.0.0.jar --task-id oz_example
```

---

## Пример 3: Планировщик (ежедневно в 2:00)

### Конфиг tasks.json

```json
{
  "tasks": [
    {
      "id": "scheduled_wb",
      "name": "Ежедневный сбор Wildberries",
      "marketplace": "WILDBERRIES",
      "productUrl": "https://www.wildberries.ru/catalog/215638903",
      "startDate": "2026-01-01",
      "endDate": "2026-12-31",
      "outputFile": "output/daily_wb_reviews.csv",
      "downloadPhotos": true,
      "photosDirectory": "output/photos/wb_daily"
    },
    {
      "id": "scheduled_oz",
      "name": "Ежедневный сбор Ozon",
      "marketplace": "OZON",
      "productUrl": "https://www.ozon.ru/product/123456789/",
      "startDate": "2026-01-01",
      "endDate": "2026-12-31",
      "outputFile": "output/daily_oz_reviews.csv",
      "downloadPhotos": true,
      "photosDirectory": "output/photos/oz_daily"
    }
  ],
  "schedule": {
    "enabled": true,
    "cronExpression": "0 0 2 * * ?",
    "timezone": "Europe/Moscow"
  }
}
```

### Запуск

```bash
java -jar target/review-collector-1.0.0.jar
# Приложение будет запущено в фоновом режиме и выполнять задачи каждый день в 2:00
```

---

## Пример 4: Множественные задачи

### Конфиг tasks.json

```json
{
  "tasks": [
    {
      "id": "task_1",
      "name": "Товар 1 - Wildberries",
      "marketplace": "WILDBERRIES",
      "productUrl": "https://www.wildberries.ru/catalog/111111111",
      "startDate": "2026-05-01",
      "endDate": "2026-06-06",
      "outputFile": "output/product1_wb.csv",
      "downloadPhotos": true,
      "photosDirectory": "output/photos/product1_wb"
    },
    {
      "id": "task_2",
      "name": "Товар 2 - Wildberries",
      "marketplace": "WILDBERRIES",
      "productUrl": "https://www.wildberries.ru/catalog/222222222",
      "startDate": "2026-05-01",
      "endDate": "2026-06-06",
      "outputFile": "output/product2_wb.csv",
      "downloadPhotos": true,
      "photosDirectory": "output/photos/product2_wb"
    },
    {
      "id": "task_3",
      "name": "Товар 3 - Ozon",
      "marketplace": "OZON",
      "productUrl": "https://www.ozon.ru/product/333333333/",
      "startDate": "2026-05-01",
      "endDate": "2026-06-06",
      "outputFile": "output/product3_oz.csv",
      "downloadPhotos": true,
      "photosDirectory": "output/photos/product3_oz"
    }
  ],
  "schedule": {
    "enabled": true,
    "cronExpression": "0 0 * * * ?",
    "timezone": "Europe/Moscow"
  }
}
```

### Запуск

```bash
# Выполнить все задачи сразу
java -jar target/review-collector-1.0.0.jar --run-once

# Или запустить планировщик для автоматического выполнения каждый час
java -jar target/review-collector-1.0.0.jar
```

---

## Файловая структура вывода

```
output/
├── wildberries_reviews.csv
├── ozon_reviews.csv
└── photos/
    ├── wb/
    │   ├── [uuid-1].jpg
    │   ├── [uuid-2].jpg
    │   └── ...
    └── oz/
        ├── [uuid-1].jpg
        ├── [uuid-2].jpg
        └── ...
```

---

## Чтение CSV результатов

### Python

```python
import pandas as pd

df = pd.read_csv('output/wildberries_reviews.csv')
print(df.head())
print(f"Всего отзывов: {len(df)}")
print(f"Средний рейтинг: {df['rating'].mean():.2f}")
```

### Excel

1. Откройте Excel
2. Выберите Файл → Открыть
3. Выберите `output/wildberries_reviews.csv`

---

## Советы

✅ Используйте разные ID задач для разных товаров  
✅ Периодически архивируйте старые CSV файлы  
✅ Проверяйте логи в `logs/` директории  
✅ Для большого количества задач используйте планировщик  
