# Marketplace Review Collector - Getting Started Guide

## Быстрый старт

### 1. Подготовка

Перед запуском убедитесь, что установлены:
- Java 11+
- Maven 3.6+

### 2. Клонирование и сборка

```bash
git clone https://github.com/murmurano-star/marketplace-review-collector.git
cd marketplace-review-collector
mvn clean package
```

### 3. Конфигурирование задач

Отредактируйте `src/main/resources/tasks.json`:

```json
{
  "tasks": [
    {
      "id": "task_1",
      "name": "Сбор отзывов Wildberries",
      "marketplace": "WILDBERRIES",
      "productUrl": "https://www.wildberries.ru/catalog/123456789",
      "startDate": "2026-05-01",
      "endDate": "2026-06-06",
      "outputFile": "reviews_wb.csv",
      "downloadPhotos": true,
      "photosDirectory": "photos/wb"
    }
  ],
  "schedule": {
    "enabled": false,
    "cronExpression": "0 0 2 * * ?",
    "timezone": "Europe/Moscow"
  }
}
```

### 4. Запуск

**Один раз:**
```bash
java -jar target/review-collector-1.0.0.jar --run-once
```

**С расписанием:**
```bash
java -jar target/review-collector-1.0.0.jar
```

## Извлечение ID товара из URL

### Wildberries
URL: `https://www.wildberries.ru/catalog/123456789`  
ID: `123456789`

### Ozon
URL: `https://www.ozon.ru/product/123456789/`  
ID: `123456789`

## Расписание Cron

- `0 0 2 * * ?` - Каждый день в 2:00 ночи
- `0 0 * * * ?` - Каждый час в начале часа
- `0 0 0 ? * MON` - Каждый понедельник в полночь
- `0 0 */6 * * ?` - Каждые 6 часов
- `0 0 2 ? * MON-FRI` - Каждый будний день в 2:00

## CSV Output

Файл содержит колонки:
- `id` - ID отзыва
- `marketplace` - Источник (WILDBERRIES/OZON)
- `product_id` - ID товара
- `author` - Автор отзыва
- `rating` - Рейтинг (1-5)
- `title` - Заголовок
- `text` - Текст отзыва
- `date` - Дата публикации
- `photos` - URL фото (разделены ;)
- `helpful_count` - Полезно
- `collected_at` - Время сбора

## Решение проблем

### Ошибка: "Не удалось извлечь ID товара"
✅ Проверьте, что URL содержит правильный ID товара

### Ошибка: "Задача не найдена"
✅ Убедитесь, что ID задачи существует в tasks.json

### Фото не загружаются
✅ Убедитесь, что `downloadPhotos: true` в конфиге

### Планировщик не запускается
✅ Установите `"schedule": { "enabled": true }`

## Логи

Логи сохраняются в:
- `logs/application.log` - Основной лог
- `logs/error.log` - Только ошибки

## Поддержка

Создавайте Issues в репозитории для вопросов и проблем!
