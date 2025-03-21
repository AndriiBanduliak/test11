# Trading Journal App

Trading Journal App – это веб-приложение для ведения журнала трейдинга, которое позволяет пользователям отслеживать свои сделки, баланс и управлять учетными данными через единый интерфейс. Приложение интегрируется с несколькими биржами (например, Binance, Bitget, CBPro, Mexc, Kucoin, Kraken) посредством отдельных микросервисов, работающих в Docker-контейнерах.

---

## 1. Project Overview

**Trading Journal App** предоставляет следующие возможности:
- **Регистрация и аутентификация:** Пользователи могут зарегистрироваться, указав необходимые данные (в том числе дату рождения, которая фиксируется и не может быть изменена) и войти в систему с помощью JWT.
- **Управление учетными данными:** Пользователь может обновлять свою персональную информацию (email, телефон) и вводить API‑ключи для подключения к различным криптобиржам.
- **Интеграция с биржами:** При наличии введенных API‑данных из бирж (Binance, Bitget, CBPro и т.д.) автоматически запрашиваются актуальные балансы и отображается торговая статистика.
- **Многоязычный интерфейс:** Поддерживаются английский, русский, немецкий и испанский языки. Все ключевые надписи и подсказки меняются в зависимости от выбранной локали.
- **Отображение торговых метрик:** Пользователь видит сводную информацию о капитале, историю сделок, автоматические сделки (выгрузка из API) и диаграмму изменения капитала.

---

## 2. Architecture

Проект построен с использованием микросервисной архитектуры и контейнеризации:

- **Main Service (Backend):**  
  Основной сервер на FastAPI, отвечающий за бизнес-логику, маршруты, аутентификацию, работу с базой данных (SQLite) через SQLAlchemy, рендеринг HTML-шаблонов (Jinja2) и локализацию.

- **Микросервисы бирж:**  
  Каждый микросервис (Binance, Bitget, CBPro, Mexc, Kucoin, Kraken) реализован как отдельный FastAPI-сервис, который принимает HTTP-запросы для получения данных с конкретной биржи через соответствующий API.

- **База данных:**  
  Используется SQLite, расположенная в каталоге `data` (DATABASE_URL: `sqlite:///./data/test.db`), для хранения данных пользователей, API-ключей, сделок и другой информации.

- **Контейнеризация:**  
  Все сервисы запускаются в Docker-контейнерах, координируемых с помощью Docker Compose.

- **Локализация:**  
  Реализована система многоязычных переводов, позволяющая динамически изменять язык интерфейса в зависимости от параметра запроса.

---

## 3. Requirements

### Системные требования:
- **Операционная система:** Linux, Windows или macOS с поддержкой Docker.
- **Docker и Docker Compose:**  
  - Docker – для создания и запуска контейнеров.
  - Docker Compose – для оркестрации всех сервисов.

### Программные зависимости:
- **Python 3.9+** – основной язык разработки.
- **FastAPI** – веб-фреймворк для создания REST API.
- **Uvicorn** – ASGI сервер для FastAPI.
- **SQLAlchemy** – ORM для работы с базой данных.
- **Pydantic** – для валидации данных и схем.
- **Jinja2** – для шаблонизации HTML.
- **python-dotenv** – для загрузки переменных окружения.
- **passlib** и **python-jose** – для хэширования паролей и работы с JWT.
- Дополнительные библиотеки для работы с API бирж (например, `cbpro` для Coinbase Pro, `krakenex` для Kraken, `pybitget` или `python-bitget` для Bitget и т.д.).

### Переменные окружения:
- **DATABASE_URL:** путь к базе данных (например, `sqlite:///./data/test.db`).
- **SECRET_KEY:** секретный ключ для генерации JWT.
- API‑ключи и секреты для каждой биржи (например, `BINANCE_API_KEY`, `BITGET_API_KEY`, `CBPRO_API_KEY` и т.д.).

---

## 4. Installation and Deployment

### Клонирование репозитория
```
bash
git clone <repository-url>
cd <repository-folder>

cp .env.example .env
```
### Обновите следующие переменные:

```
DATABASE_URL – например, sqlite:///./data/test.db 
SECRET_KEY – ваш секретный ключ для JWT
API‑ключи для бирж: 
BINANCE_API_KEY, 
BINANCE_SECRET_KEY, 
BITGET_API_KEY, 
BITGET_SECRET_KEY, 
BITGET_PASSPHRASE, 
CBPRO_API_KEY, 
CBPRO_SECRET_KEY, 
CBPRO_PASSPHRASE и т.д. 
```
## 5.Сборка и запуск контейнеров
Убедитесь, что Docker и Docker Compose установлены, затем выполните:
```
bash
docker-compose up --build
```
После сборки основной сервис будет доступен по адресу: http://127.0.0.1:8000.

## 6. Configuration
### Структура проекта

### backend/ – основной сервис, содержащий:
```
FastAPI-приложение
Модели (SQLAlchemy)
Маршруты и аутентификацию
HTML-шаблоны (Jinja2)
services/ – микросервисы для бирж (например, Binance, Bitget, CBPro и т.д.)
data/ – каталог для хранения базы данных (например, SQLite)
.env – файл с переменными окружения
docker-compose.yaml – файл для сборки и запуска контейнеров
```
### Переменные окружения
```
DATABASE_URL: sqlite:///./data/test.db
SECRET_KEY: ваш секретный ключ для JWT
API‑ключи для бирж:
BINANCE_API_KEY и BINANCE_SECRET_KEY
BITGET_API_KEY, BITGET_SECRET_KEY, BITGET_PASSPHRASE
CBPRO_API_KEY, CBPRO_SECRET_KEY, CBPRO_PASSPHRASE
и т.д.
```
