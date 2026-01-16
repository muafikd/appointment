# Appointment Management System

Система управления записями клиентов на услуги

## Описание проекта

Простая система для управления записями клиентов на услуги. Клиенты могут регистрироваться, просматривать доступные временные слоты и записываться на услуги.

### Основные возможности:

- ✅ Регистрация клиентов
- ✅ Управление услугами
- ✅ Управление расписанием доступных слотов
- ✅ Бронирование записей клиентами
- ✅ Отмена и отслеживание статуса записей
- ✅ Разделение бизнес-логики и доступа к базе через OOP-архитектуру с DAO и Service слоями
- ✅ Подключение к реляционной базе данных через JDBC

## Технологии

- **Java 17**
- **JDBC**
- **PostgreSQL** (или MySQL)
- **Maven** (управление зависимостями)

## Структура проекта

```
AppointmentManagementSystem/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── org/example/
│   │   │       ├── entity/          # Сущности (Client, Service, Appointment)
│   │   │       ├── dao/             # Слой доступа к данным (DAO)
│   │   │       ├── service/         # Слой бизнес-логики
│   │   │       └── Main.java        # CLI интерфейс для тестирования
│   │   └── resources/
│   │       ├── database.properties  # Настройки подключения к БД
│   │       └── schema.sql           # SQL скрипт для создания схемы БД
│   └── test/
├── pom.xml                          # Maven конфигурация
└── README.md
```

## Архитектура

### Entities (Сущности)
- `Client` - клиент системы
- `Service` - услуга (название, длительность, цена)
- `Appointment` - запись клиента на услугу (с датой и временем)

### DAO Layer (Слой доступа к данным)
- `ClientDAO` - CRUD операции для клиентов
- `ServiceDAO` - CRUD операции для услуг
- `AppointmentDAO` - CRUD операции для записей

### Service Layer (Слой бизнес-логики)
- `AppointmentService` - бизнес-логика для записей (проверка доступности времени, запись/отмена)

### Presentation Layer
- `Main` - CLI интерфейс для тестирования функционала

## Установка и настройка

### Требования

- Java 17 или выше
- Maven 3.6+
- PostgreSQL 12+ (или MySQL 8+)

### Шаги установки

1. **Клонируйте репозиторий:**
   ```bash
   git clone <repository-url>
   cd AppointmentManagementSystem
   ```

2. **Создайте базу данных:**
   ```sql
   CREATE DATABASE appointmentdb;
   ```

3. **Настройте подключение к базе данных:**
   
   Отредактируйте файл `src/main/resources/database.properties`:
   ```properties
   db.url=jdbc:postgresql://localhost:5432/appointmentdb
   db.user=postgres
   db.password=your_password
   ```

4. **Создайте схему базы данных:**
   
   Выполните SQL скрипт из файла `src/main/resources/schema.sql` в вашей базе данных:
   ```bash
   psql -U postgres -d appointmentdb -f src/main/resources/schema.sql
   ```
   
   Или выполните скрипт через любой SQL клиент.

5. **Соберите проект:**
   ```bash
   mvn clean compile
   ```

6. **Запустите приложение:**
   ```bash
   mvn exec:java -Dexec.mainClass="org.example.Main"
   ```
   
   Или через IDE запустите класс `org.example.Main`

## Структура базы данных

### Таблицы

#### 1. `clients` - таблица клиентов
| Поле | Тип | Описание |
|------|-----|----------|
| id | SERIAL PRIMARY KEY | уникальный идентификатор клиента |
| name | VARCHAR(100) | имя клиента |
| email | VARCHAR(100) | email (уникальный) |
| phone | VARCHAR(20) | телефон |

#### 2. `services` - таблица услуг
| Поле | Тип | Описание |
|------|-----|----------|
| id | SERIAL PRIMARY KEY | уникальный идентификатор услуги |
| name | VARCHAR(100) | название услуги |
| duration | INT | продолжительность услуги в минутах |
| price | DECIMAL(10,2) | цена услуги |

#### 3. `appointments` - таблица записей клиентов
| Поле | Тип | Описание |
|------|-----|----------|
| id | SERIAL PRIMARY KEY | уникальный идентификатор записи |
| client_id | INT REFERENCES clients(id) | клиент |
| service_id | INT REFERENCES services(id) | услуга |
| appointment_date | DATE | дата записи |
| appointment_time | TIME | время записи |
| status | VARCHAR(20) | статус записи (booked, cancelled, completed) |

## Использование

После запуска приложения вы увидите главное меню с опциями:

1. **Manage Clients** - регистрация и управление клиентами
2. **Manage Services** - управление услугами
3. **Manage Appointments** - бронирование и управление записями
4. **Exit** - выход

### Пример использования:

1. Зарегистрируйте клиента через меню "Manage Clients"
2. Создайте услуги через меню "Manage Services"
3. Запишите клиента на услугу через меню "Manage Appointments" (укажите дату и время)

## Особенности реализации

- **Валидация данных**: Проверка доступности времени для записи
- **Транзакционность**: Использование PreparedStatement для безопасных SQL запросов
- **Обработка ошибок**: Корректная обработка SQLException и бизнес-логических ошибок
- **Разделение ответственности**: Четкое разделение на слои Entity, DAO, Service, Presentation

## Разработка

### Добавление новых функций

1. Добавьте новые методы в соответствующий DAO класс
2. Добавьте бизнес-логику в Service класс
3. Обновите CLI интерфейс в Main.java

### Тестирование

Для тестирования используйте CLI интерфейс или создайте unit-тесты в директории `src/test/java`.

## Лицензия

Этот проект создан в образовательных целях.

## Автор

Appointment Management System - учебный проект

# appointment
