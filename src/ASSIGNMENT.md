# Розробка карусельного слайдера з використанням ООП в JavaScript

**Складність**: Середня

## Мета завдання

Розробити інтерактивний карусельний слайдер використовуючи об'єктно-орієнтований підхід JavaScript. Реалізація має підтримувати перемикання слайдів, автоматичне відтворення, керування клавіатурою та підтримку свайпів.

## Основні вимоги

### 1. Структура класів

- Базовий клас `Carousel` з основною функціональністю
- Розширений клас `SwipeCarousel` з підтримкою свайпів
- **ОБОВ'ЯЗКОВО**: Модуль повинен експортувати:
  ```javascript
  export { Carousel, SwipeCarousel }
  export default SwipeCarousel
  ```

### 2. Базова HTML структура

```html
<div id="carousel">
  <div class="slide active"></div>
  <div class="slide"></div>
  <div class="slide"></div>
</div>
```

### 3. Клас `Carousel`

- **Конструктор** має приймати об'єкт налаштувань:

  ```javascript
  constructor(options) {
    const settings = {
      ...DEFAULT_SETTINGS,
      ...options
    };
    // ініціалізація властивостей
  }
  ```

- **Основні властивості**:

  - `container` - DOM-елемент контейнера
  - `slides` - колекція слайдів
  - `TIMER_INTERVAL` - інтервал перемикання
  - `isPlaying` - статус відтворення
  - `pauseOnHover` - статус паузи при наведенні

- **Основні методи**:

  - `init()` - ініціалізує карусель
  - `next()` - перемикання на наступний слайд
  - `prev()` - перемикання на попередній слайд
  - `pause()` - зупинка автовідтворення
  - `play()` - запуск автовідтворення
  - `pausePlay()` - переключення між паузою та відтворенням

- **Приватні методи**:

  - `#gotoNth(n)` - приймає числовий індекс слайду і переходить на нього
  - `#indicatorClick(e)` - обробник кліку на індикатор, має конвертувати значення з `dataset.slideTo` в число (використовуючи унарний плюс або іншу техніку конвертації)

### 4. Клас `SwipeCarousel`

- Наслідує функціональність з `Carousel`
- Додає підтримку свайпів (для мобільних та десктопних пристроїв)
- Повинен обробляти два типи свайпів:
  - Touch-свайпи через події `touchstart` і `touchend`
  - Mouse-свайпи через події `mousedown` і `mouseup`

### 5. Функціональність для тестування

- **Елементи керування** (створюються динамічно):

  - Контейнер кнопок керування
  - Кнопки: `#pause-btn`, `#prev-btn`, `#next-btn`
  - Контейнер індикаторів
  - Індикатори з HTML-атрибутами `data-slide-to="n"` (які в JavaScript доступні як `dataset.slideTo`)

- **Керування клавіатурою**:

  - Клавіша Space - пауза/відтворення (з `preventDefault()`)
  - Клавіша ArrowLeft - перехід до попереднього слайду
  - Клавіша ArrowRight - перехід до наступного слайду
  - Обробник клавіатурних подій має бути приєднаний до `document`

- **Підтримка свайпів**:

  - Підтримка свайпів на мобільних і десктопних пристроях
  - Свайп вліво - перехід на наступний слайд
  - Свайп вправо - перехід на попередній слайд
  - Поріг для свайпу: 100 пікселів

- **Автоматичне перемикання**:

  - Інтервал визначається через `TIMER_INTERVAL`
  - Автоматично запускається, якщо `isPlaying === true`
  - Зупиняється при взаємодії (кліки на кнопки, індикатори)

- **Візуальна індикація**:

  - Активний слайд має клас `active`
  - Активний індикатор має клас `active`
  - Іконка пауза/відтворення змінюється

- **Обробка типів даних**:

  - Індикатори мають HTML-атрибути `data-slide-to="n"`, які доступні в JavaScript як `dataset.slideTo`
  - Значення цих атрибутів отримуються як рядки, навіть якщо вони містять числа
  - При передачі в методи для навігації слайдами ці значення повинні бути конвертовані в числа
  - Використовуйте унарний плюс (+) або інші способи конвертації (parseInt, Number)

## Критерії тестування

1. **Імпорти і експорти**:

   - Класи `Carousel` і `SwipeCarousel` доступні для імпорту

2. **Ініціалізація**:

   - Перший слайд та індикатор мають клас `active` після ініціалізації
   - Таймер автоматичного перемикання запускається, якщо `isPlaying === true`

3. **Елементи керування**:

   - Кнопки перемикання переводять до відповідних слайдів
   - Індикатори дозволяють переходити до відповідних слайдів

4. **Циклічність**:

   - При переході з останнього слайду вперед відбувається перехід до першого
   - При переході з першого слайду назад відбувається перехід до останнього

5. **Клавіатура**:

   - Клавіші керування викликають відповідні дії
   - Space викликає preventDefault()

6. **Пауза/відтворення**:

   - Кнопка паузи зупиняє таймер і змінює іконку
   - Повторне натискання запускає таймер і повертає оригінальну іконку

7. **Свайпи**:

   - Свайпи вліво/вправо викликають перехід до відповідного слайду
   - Працює як з подіями миші, так і з сенсорними подіями

8. **Налаштування**:

   - Конструктор правильно застосовує користувацькі налаштування
   - Параметр `interval` контролює швидкість автоматичного перемикання
   - Параметр `isPlaying` визначає, чи буде запущено таймер

9. **Вимоги до коду**:

   - Код повинен працювати з динамічною кількістю слайдів, використовуючи змінні замість фіксованих чисел
   - Правильна обробка типів даних: рядкові значення з атрибутів `data-slide-to` повинні конвертуватись у числа перед використанням
   - Формула для обчислення індексу наступного слайду має враховувати загальну кількість слайдів для циклічної навігації
