# Project Plan Documentation

## Project Title

Appointment Management System

## Team Members and Role Distribution

**Team Member:** [Your Name]  
**Role:** Full-Stack Developer  
**Responsibilities:**
- Database design and implementation
- Backend development (Java, JDBC)
- Business logic implementation
- CLI interface development
- Project documentation

## Project Description

### Overview

The Appointment Management System is a Java-based application designed to manage client appointments for services. The system provides a streamlined solution for registering clients, managing services, and booking appointments with specific dates and times. The application follows object-oriented programming principles with a clear separation of concerns through layered architecture (Entity, DAO, Service, Presentation layers).

### Goals and Objectives

**Primary Goals:**
1. Provide a reliable system for client registration and management
2. Enable efficient service management (creation, update, deletion)
3. Facilitate appointment booking with date and time validation
4. Support appointment status management (booked, cancelled, completed)
5. Ensure data integrity through proper database design and constraints

**Technical Objectives:**
1. Implement clean architecture with separation of concerns
2. Utilize JDBC for database connectivity
3. Apply object-oriented design principles
4. Ensure code maintainability and scalability
5. Provide a user-friendly CLI interface

### Novelty and Uniqueness

While appointment management systems are common, this project demonstrates:
- Clean layered architecture suitable for educational purposes
- Direct integration with PostgreSQL using JDBC without ORM frameworks
- Simplified design focusing on core functionality
- Clear separation between data access, business logic, and presentation layers

## Technologies and Tools

### Core Technologies

**Java 17**
- Purpose: Primary programming language for application development
- Version: Java 17 (LTS)
- Justification: Modern Java features, long-term support, compatibility with current development tools

**PostgreSQL**
- Purpose: Relational database management system for data persistence
- Version: PostgreSQL 12 or higher
- Justification: Robust ACID compliance, excellent performance, open-source, widely used in enterprise applications

**JDBC (Java Database Connectivity)**
- Purpose: API for connecting Java applications to relational databases
- Version: Included in Java SE (java.sql package)
- Justification: Direct database access without additional frameworks, full control over SQL queries, educational value

### Build and Dependency Management

**Apache Maven**
- Purpose: Project build automation and dependency management
- Version: Maven 3.6 or higher
- Justification: Standard tool for Java projects, simplifies dependency management, integrates with IDEs

**PostgreSQL JDBC Driver**
- Purpose: Database driver for PostgreSQL connectivity
- Version: 42.6.0
- Justification: Official PostgreSQL JDBC driver, maintained and updated regularly

### Development Tools

**Git**
- Purpose: Version control system
- Version: Latest stable
- Justification: Industry standard for version control, enables collaboration

**GitHub**
- Purpose: Remote repository hosting and collaboration platform
- Justification: Free hosting, integration with development workflows

## Project Structure and Class Interactions

### Architecture Overview

The project follows a layered architecture pattern:

```
┌─────────────────────────────────────┐
│     Presentation Layer (Main)       │
│         - CLI Interface             │
└─────────────────┬───────────────────┘
                  │
┌─────────────────▼───────────────────┐
│      Service Layer                  │
│  - AppointmentService               │
│  - Business Logic                   │
└─────────────────┬───────────────────┘
                  │
┌─────────────────▼───────────────────┐
│        DAO Layer                    │
│  - ClientDAO                         │
│  - ServiceDAO                        │
│  - AppointmentDAO                   │
│  - DatabaseConnection                │
└─────────────────┬───────────────────┘
                  │
┌─────────────────▼───────────────────┐
│      Entity Layer                   │
│  - Client                            │
│  - Service                           │
│  - Appointment                       │
└─────────────────┬───────────────────┘
                  │
┌─────────────────▼───────────────────┐
│      Database (PostgreSQL)           │
│  - clients table                     │
│  - services table                    │
│  - appointments table                │
└─────────────────────────────────────┘
```

### Class Diagram

```
┌──────────────┐
│   Client     │
├──────────────┤
│ - id         │
│ - name       │
│ - email      │
│ - phone      │
└──────────────┘

┌──────────────┐
│   Service    │
├──────────────┤
│ - id         │
│ - name       │
│ - duration   │
│ - price      │
└──────────────┘

┌──────────────┐
│ Appointment  │
├──────────────┤
│ - id         │
│ - clientId   │
│ - serviceId  │
│ - date       │
│ - time       │
│ - status     │
└──────────────┘
      │
      │ uses
      ▼
┌──────────────┐
│ Appointment  │
│   Service    │
├──────────────┤
│ + bookAppt() │
│ + cancel()   │
│ + complete() │
└──────────────┘
      │
      │ uses
      ▼
┌──────────────┐
│ AppointmentDAO│
├──────────────┤
│ + create()   │
│ + findById() │
│ + update()   │
│ + delete()   │
└──────────────┘
```

### Activity Diagram: Booking an Appointment

```
[Start] → [Display Menu] → [User Selects Book Appointment]
    ↓
[Enter Client ID] → [Enter Service ID] → [Enter Date] → [Enter Time]
    ↓
[AppointmentService.bookAppointment()]
    ↓
[Check Service Exists?] → No → [Error: Service Not Found] → [End]
    ↓ Yes
[Check Time Slot Available?] → No → [Error: Time Slot Booked] → [End]
    ↓ Yes
[Create Appointment] → [Save to Database] → [Display Success] → [End]
```

### Database Schema

**clients table:**
- id (SERIAL PRIMARY KEY)
- name (VARCHAR(100) NOT NULL)
- email (VARCHAR(100) UNIQUE NOT NULL)
- phone (VARCHAR(20) NOT NULL)

**services table:**
- id (SERIAL PRIMARY KEY)
- name (VARCHAR(100) NOT NULL)
- duration (INT NOT NULL, CHECK > 0)
- price (DECIMAL(10,2) NOT NULL, CHECK >= 0)

**appointments table:**
- id (SERIAL PRIMARY KEY)
- client_id (INT REFERENCES clients(id))
- service_id (INT REFERENCES services(id))
- appointment_date (DATE NOT NULL)
- appointment_time (TIME NOT NULL)
- status (VARCHAR(20) DEFAULT 'booked', CHECK IN ('booked', 'cancelled', 'completed'))

## Minimum Viable Product (MVP)

### Core Features

**1. Client Management**
- **Feature:** Register new clients with name, email, and phone
- **Implementation:** ClientDAO provides CRUD operations. Main class provides CLI interface for client registration, listing, searching, updating, and deletion
- **Purpose:** Foundation for appointment booking - clients must exist before booking

**2. Service Management**
- **Feature:** Create and manage services with name, duration, and price
- **Implementation:** ServiceDAO handles database operations. CLI allows service creation, listing, searching, updating, and deletion
- **Purpose:** Services represent what clients can book appointments for

**3. Appointment Booking**
- **Feature:** Book appointments by selecting client, service, date, and time
- **Implementation:** AppointmentService validates service existence and time slot availability. AppointmentDAO persists appointment data
- **Business Logic:** Prevents double-booking by checking if time slot is already booked
- **Purpose:** Core functionality - enables clients to schedule appointments

**4. Appointment Status Management**
- **Feature:** View, cancel, and complete appointments
- **Implementation:** AppointmentService enforces business rules (cannot cancel completed appointments, only booked appointments can be completed)
- **Status Flow:** booked → cancelled OR booked → completed
- **Purpose:** Track appointment lifecycle

**5. Data Validation**
- **Feature:** Input validation and error handling
- **Implementation:** Service layer validates business rules, database constraints enforce data integrity
- **Purpose:** Ensure data quality and prevent invalid operations

### MVP Implementation Details

**Database Layer:**
- Three core tables with proper relationships and constraints
- Indexes on foreign keys and frequently queried columns
- Cascade delete to maintain referential integrity

**Data Access Layer (DAO):**
- PreparedStatement for SQL injection prevention
- Connection pooling through DatabaseConnection singleton
- Proper resource management with try-with-resources

**Service Layer:**
- Business logic encapsulation
- Validation before database operations
- Exception handling with meaningful error messages

**Presentation Layer:**
- Menu-driven CLI interface
- User-friendly prompts and error messages
- Input validation and error recovery

## Post-MVP Improvements

### Phase 1: Enhanced Features

**1. Appointment Conflict Detection**
- Check for overlapping appointments considering service duration
- Prevent booking if service duration would conflict with existing appointments

**2. Search and Filtering**
- Search appointments by date range
- Filter appointments by status
- Find available time slots for a specific date

**3. Email Notifications**
- Send confirmation emails when appointments are booked
- Send reminders before appointment date
- Notify on appointment cancellation

### Phase 2: User Interface Improvements

**1. Web Interface**
- Replace CLI with web-based interface using Java Servlets or Spring Boot
- Responsive design for mobile devices
- Real-time availability updates

**2. Admin Dashboard**
- Statistics and analytics
- Appointment reports
- Service performance metrics

### Phase 3: Advanced Features

**1. Multi-user Support**
- User authentication and authorization
- Role-based access control (admin, staff, client)
- Session management

**2. Calendar Integration**
- Visual calendar view of appointments
- Drag-and-drop appointment rescheduling
- Export to external calendar applications

**3. Payment Integration**
- Payment processing for services
- Invoice generation
- Payment history tracking

**4. Notification System**
- SMS notifications
- Push notifications
- Automated reminder system

### Phase 4: Scalability and Performance

**1. Caching**
- Implement caching layer for frequently accessed data
- Reduce database load

**2. Connection Pooling**
- Advanced connection pooling (HikariCP)
- Better resource management

**3. Logging and Monitoring**
- Comprehensive logging system
- Performance monitoring
- Error tracking

## Development Timeline

### Week 1: Project Setup and Database Design
- **Tasks:**
  - Project initialization with Maven
  - Database schema design
  - Entity classes creation
  - Database connection setup
- **Deliverables:** Database schema, entity classes, connection utility

### Week 2: Data Access Layer
- **Tasks:**
  - Implement ClientDAO
  - Implement ServiceDAO
  - Implement AppointmentDAO
  - Unit testing of DAO methods
- **Deliverables:** Complete DAO layer with CRUD operations

### Week 3: Business Logic Layer
- **Tasks:**
  - Implement AppointmentService
  - Business rule validation
  - Error handling
  - Service layer testing
- **Deliverables:** Service layer with business logic

### Week 4: Presentation Layer and Integration
- **Tasks:**
  - CLI interface development
  - Integration of all layers
  - End-to-end testing
  - Bug fixes
- **Deliverables:** Working MVP application

### Week 5: Documentation and Deployment
- **Tasks:**
  - Project documentation
  - Code cleanup and optimization
  - GitHub repository setup
  - Final testing and validation
- **Deliverables:** Complete project with documentation

### Milestones

- **Milestone 1 (End of Week 1):** Database and entities ready
- **Milestone 2 (End of Week 2):** Data access layer complete
- **Milestone 3 (End of Week 3):** Business logic implemented
- **Milestone 4 (End of Week 4):** MVP functional
- **Milestone 5 (End of Week 5):** Project complete and documented

## GitHub Repository

**Repository URL:** https://github.com/muafikd/appointment.git

**Repository Structure:**
```
AppointmentManagementSystem/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── org/example/
│   │   │       ├── entity/
│   │   │       ├── dao/
│   │   │       ├── service/
│   │   │       └── Main.java
│   │   └── resources/
│   │       ├── database.properties
│   │       └── schema.sql
│   └── test/
├── pom.xml
├── README.md
└── PROJECT_PLAN.md
```

## References

American Psychological Association. (2020). *Publication manual of the American Psychological Association* (7th ed.). American Psychological Association.

Oracle Corporation. (2023). *Java Platform, Standard Edition Documentation*. Retrieved from https://docs.oracle.com/javase/

PostgreSQL Global Development Group. (2023). *PostgreSQL Documentation*. Retrieved from https://www.postgresql.org/docs/

---

**Document Version:** 1.0  
**Last Updated:** [Current Date]  
**Author:** [Your Name]
