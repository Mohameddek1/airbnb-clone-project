# Airbnb Database ERD Requirements

## Objective

Design an Entity-Relationship Diagram (ERD) for an Airbnb-like application database system. The ERD must clearly identify all entities, their attributes, primary/foreign keys, and relationships.

## Tools

* Tool Used: Draw\.io
* File Location: `ERD/database_er.drawio.png`

---

## Entities, Attributes, and Constraints

### 1. **User**

| Attribute      | Data Type | Constraints                  |
| -------------- | --------- | ---------------------------- |
| user\_id       | UUID      | Primary Key, Indexed         |
| first\_name    | VARCHAR   | NOT NULL                     |
| last\_name     | VARCHAR   | NOT NULL                     |
| email          | VARCHAR   | UNIQUE, NOT NULL             |
| password\_hash | VARCHAR   | NOT NULL                     |
| phone\_number  | VARCHAR   | NULLABLE                     |
| role           | ENUM      | guest, host, admin; NOT NULL |
| created\_at    | TIMESTAMP | DEFAULT CURRENT\_TIMESTAMP   |

### 2. **Property**

| Attribute     | Data Type | Constraints                   |
| ------------- | --------- | ----------------------------- |
| property\_id  | UUID      | Primary Key, Indexed          |
| host\_id      | UUID      | Foreign Key -> User(user\_id) |
| name          | VARCHAR   | NOT NULL                      |
| description   | TEXT      | NOT NULL                      |
| location      | VARCHAR   | NOT NULL                      |
| pricepernight | DECIMAL   | NOT NULL                      |
| created\_at   | TIMESTAMP | DEFAULT CURRENT\_TIMESTAMP    |
| updated\_at   | TIMESTAMP | ON UPDATE CURRENT\_TIMESTAMP  |

### 3. **Booking**

| Attribute    | Data Type | Constraints                            |
| ------------ | --------- | -------------------------------------- |
| booking\_id  | UUID      | Primary Key, Indexed                   |
| property\_id | UUID      | Foreign Key -> Property(property\_id)  |
| user\_id     | UUID      | Foreign Key -> User(user\_id)          |
| start\_date  | DATE      | NOT NULL                               |
| end\_date    | DATE      | NOT NULL                               |
| total\_price | DECIMAL   | NOT NULL                               |
| status       | ENUM      | pending, confirmed, canceled; NOT NULL |
| created\_at  | TIMESTAMP | DEFAULT CURRENT\_TIMESTAMP             |

### 4. **Payment**

| Attribute       | Data Type | Constraints                            |
| --------------- | --------- | -------------------------------------- |
| payment\_id     | UUID      | Primary Key, Indexed                   |
| booking\_id     | UUID      | Foreign Key -> Booking(booking\_id)    |
| amount          | DECIMAL   | NOT NULL                               |
| payment\_date   | TIMESTAMP | DEFAULT CURRENT\_TIMESTAMP             |
| payment\_method | ENUM      | credit\_card, paypal, stripe; NOT NULL |

### 5. **Review**

| Attribute    | Data Type | Constraints                              |
| ------------ | --------- | ---------------------------------------- |
| review\_id   | UUID      | Primary Key, Indexed                     |
| property\_id | UUID      | Foreign Key -> Property(property\_id)    |
| user\_id     | UUID      | Foreign Key -> User(user\_id)            |
| rating       | INTEGER   | CHECK (rating BETWEEN 1 AND 5); NOT NULL |
| comment      | TEXT      | NOT NULL                                 |
| created\_at  | TIMESTAMP | DEFAULT CURRENT\_TIMESTAMP               |

### 6. **Message**

| Attribute     | Data Type | Constraints                   |
| ------------- | --------- | ----------------------------- |
| message\_id   | UUID      | Primary Key, Indexed          |
| sender\_id    | UUID      | Foreign Key -> User(user\_id) |
| recipient\_id | UUID      | Foreign Key -> User(user\_id) |
| message\_body | TEXT      | NOT NULL                      |
| sent\_at      | TIMESTAMP | DEFAULT CURRENT\_TIMESTAMP    |

---

## Relationships

* A `User` can **host** multiple `Properties`.
* A `User` can **book** multiple `Properties` via `Booking`.
* A `User` can **write** multiple `Reviews`.
* A `User` can **send and receive** multiple `Messages`.
* A `Property` can have **multiple Bookings** and **multiple Reviews**.
* A `Booking` has **one Payment**.

---

## Indexing Strategy

* Primary keys are indexed automatically.
* Additional indexes:

  * `User.email`
  * `Property.property_id`
  * `Booking.property_id`, `Booking.user_id`
  * `Payment.booking_id`

---

