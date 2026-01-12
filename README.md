# DEPOC 

A comprehensive management system built by a business owner, tailored for small Brazilian businesses.

This project took shape in response to a specific need in my last brick-and-mortar store.

# Contents

- [API Reference](#api-reference)
  - [Authentication](#authentication)
  - [Errors](#errors)
  - [Pagination](#pagination)
  - [Search](#search)
  - [Date](#date)
  - [Rate Limite](#rate-limit)
  - [Core Resources](#core-resources)
- [Stack](#stack)

# API Reference

The system exposes a RESTful interface at the root endpoint https://api.depoc.com.br, serving as the primary access point for client interaction. It uses resource-oriented URLs, and operates with JSON HTTP requests and responses.

## Authentication

The Depoc API uses JWT (JSON Web Token) to authenticate requests.

```bash 
curl https://api.depoc.com.br/me \
-H "Authorization: Bearer YOUR_TOKEN" \
-H "Content-Type: application/json"
```

## Errors

Depoc uses standard HTTP status codes to signal the outcome of API requests:

- 2xx: Success
- 4xx: Client error (e.g., invalid request parameters)
- 5xx: Server error (Depoc-side failure)

```json
  "error": {
      "status": 400,
      "message": "Validation failed.",
      "details": {
          "username": [
              "user with this username already exists."
          ]
      }
  }
```

**Error Response Format**
- `status` *int*

  The HTTP status code.

- `message` *str*

  Message describing the error.

- `details` *object*

  Provide detailed error information. Especially useful for client-side issues such as invalid parameters.


## Pagination

All API resources that may return large datasets are paginated (e.g., contacts, transactions). Each page returns a maximum of 50 items.

```json
"count": 502,
"next": "https://api.depoc.com.br/finance/transactions?page=2",
"previous": null,
"results": [
    {
        "transaction": {
            "id": "01JYM6QVGZBSTGMECFQV138WAA",
            "description": "First Sale",
            "amount": "300.00",
            "type": "credit",
            "timestamp": "2025-06-25T15:48:55.075622-03:00",
            "category": {
                "id": "01JMZ6BJ42087F10C59CGZ9749",
                "name": "Sales"
            },
            "operator": {
                "id": "01JM18P43G7QPHF07X67P0REAS",
                "name": "Alice"
            },
            "account": {
                "id": "01JRJNG5G7Y02ZPV3F2LQYCB8A",
                "name": "Wise"
            },
            "contact": {
                "name": null,
                "id": null
            },
            "payment": null,
            "linked": null
        }
    }
]
```

**Parameters**

- `page` *int*

   The page number to retrieve in a paginated response (e.g., `/finance/transactions?page=2`)

**Pagination Response Format**

- `count` *int*

   The number of results returned.

- `next` *url*

   The URL for the next page.

- `previous` *url*

   The URL for the previous page.

- `results` *array*

   An array containing the response elements.

## Search

Some API resources have support for retrieval via search parameter.

**Parameters**

- `search` *str*
  The search term longer the three characters.

`https://api.depoc.com.br/contacts?search=supp`

```json
"count": 1,
"next": null,
"previous": null,
"results": [
    {
        "supplier": {
            "id": "01JV0RQS2AN6EN7AS2XHEPB9JE",
            "code": null,
            "legal_name": "Supplier Inc",
            "trade_name": "Supp",
            "cnpj": null,
            "ie": null,
            "im": null,
            "is_active": true,
            "notes": null,
            "phone": null,
            "email": null,
            "postcode": null,
            "city": null,
            "state": null,
            "address": null,
            "created_at": "2025-05-11T19:51:32.811784-03:00",
            "updated_at": "2025-05-11"
        }
    }
]
```

## Date

Some API resources support filtering results by date.

**Parameters**

- `date` *str*

  The `date` parameter exclusively accepts the following shortcuts: `today`, `week`, and `month`, in addition to the standard date format "YYYY-MM-DD".

- `start_date` & `end_date` *str*

   Must be paired together and must follow the "YYYY-MM-DD" format.

The date and search parameters can be used combined (e.g., `/?date=today&search=bags`)

## Rate Limit
The global rate limit is 60 requests per minute and up to 1,000 requests per day.

## Exemple Resources

### `/me`

Retrieve current user's data.

GET `https://api.depoc.com.br/me`

```json
"user": {
    "id": "01JM60P43G1QPHF00X67P01LAS",
    "name": "The User",
    "email": "user@email.com",
    "username": "theuser",
    "is_active": true,
    "is_staff": true,
    "last_login": "2025-04-29T20:23:39.909901-03:00",
    "date_joined": "2025-02-15T23:16:34.529502-03:00"
}
```
<details>

<summary>Attributes</summary>

- **id** *str*

  Unique ID of the user.

  Exemple: `01JM60P43G1QPHF00X67P01LAS`

- **name** *str*

  Name of the user.

  Exemple: `Sample User`

- **email** *str*

  Email of the user.

  Exemple: `user@email.com`

- **username** *str*

  Username of the user

  Exemple: `theusername`

- **is_active** *bool*

- **is_staff** *bool*

- **last_login** *str*

  Time of last login.

  Exemple: `2025-04-29T20:23:39.909901-03:00`

- **date_joined** *str*

  Date which the user registered.

  Exemple:  `2025-02-15T23:16:34.529502-03:00`

</details>

### `/accounts`

Create a new account.

POST `https://api.depoc.com.br/accounts`

```json
"user": {
    "name": "The User",
    "username": "theuser",
    "email": "user@email.com",
    "password": "secret",
}
```
<details>

<summary>Attributes</summary>

- **name** *str*

  Name of the user.

  Exemple: `Sample User`

- **username** *str*

  Username of the user

  Exemple: `theusername`

- **email** *str*

  Email of the user.

  Exemple: `user@email.com`

- **password** *str*

  Password of the user.

  Exemple: `!secretpassword`

</details>

### `/contacts`

Retrieve all contacts.

GET `https://api.depoc.com.br/contacts`

```json
{
    "count": 2,
    "next": null,
    "previous": null,
    "results": [
        {
            "customer": {
                "id": "01JN6Y34TSBZX7415DAZK7A1QC",
                "code": "1",
                "name": "Customer",
                "alias": "C1",
                "gender": "male",
                "cpf": "01922875389",
                "is_active": true,
                "notes": "",
                "phone": "11989283948",
                "email": "customer@email.com",
                "postcode": "47090000",
                "city": "São Paulo",
                "state": "SP",
                "address": "Avenida",
                "amount_spent": "0.00",
                "number_of_orders": 0,
                "created_at": "2025-02-28T15:46:23.836267-03:00",
                "updated_at": "2025-04-30"
            }
        },
        {
            "supplier": {
                "id": "01JV0RQS2AN0EN7AS2XHEPA7JE",
                "code": 2,
                "legal_name": "Supplier Inc",
                "trade_name": "The Supplier Shop",
                "cnpj": null,
                "ie": null,
                "im": null,
                "is_active": true,
                "notes": null,
                "phone": null,
                "email": null,
                "postcode": null,
                "city": null,
                "state": null,
                "address": null,
                "created_at": "2025-05-11T19:51:32.811784-03:00",
                "updated_at": "2025-05-11"
            }
        }
    ]
}
```
<details>

<summary>Customer Attributes</summary>

- **id** *str*  
  Unique identifier of the customer.  
  Example: `01JN6Y34TSBZX7415DAZK7A1QC`

- **code** *str*  
  Customer code.  
  Example: `1`

- **name** *str*  
  Full name of the customer.  
  Example: `Customer`

- **alias** *str*  
  Short name or nickname.  
  Example: `C1`

- **gender** *str*  
  Gender of the customer.  
  Example: `male`

- **cpf** *str*  
  Brazilian CPF number.  
  Example: `01922875389`

- **is_active** *bool*  
  Whether the customer is active.  
  Example: `true`

- **notes** *str*  
  Additional notes.  
  Example: `""`

- **phone** *str*  
  Phone number of the customer.  
  Example: `11989283948`

- **email** *str*  
  Email address.  
  Example: `customer@email.com`

- **postcode** *str*  
  Postal code.  
  Example: `47090000`

- **city** *str*  
  City of residence.  
  Example: `São Paulo`

- **state** *str*  
  State of residence.  
  Example: `SP`

- **address** *str*  
  Street address.  
  Example: `Avenida`

- **amount_spent** *str*  
  Total amount spent.  
  Example: `"0.00"`

- **number_of_orders** *int*  
  Total number of orders.  
  Example: `0`

- **created_at** *str*  
  Creation timestamp.  
  Example: `2025-02-28T15:46:23.836267-03:00`

- **updated_at** *str*  
  Last update timestamp.  
  Example: `2025-04-30`

</details>

<details>

<summary>Supplier Attributes</summary>

- **id** *str*  
  Unique identifier of the supplier.  
  Example: `01JV0RQS2AN0EN7AS2XHEPA7JE`

- **code** *int*  
  Supplier code.  
  Example: `2`

- **legal_name** *str*  
  Legal name of the supplier.  
  Example: `Supplier Inc`

- **trade_name** *str*  
  Commercial name.  
  Example: `The Supplier Shop`

- **cnpj** *str | null*  
  Brazilian CNPJ number.  
  Example: `null`

- **ie** *str | null*  
  State registration.  
  Example: `null`

- **im** *str | null*  
  Municipal registration.  
  Example: `null`

- **is_active** *bool*  
  Whether the supplier is active.  
  Example: `true`

- **notes** *str | null*  
  Additional notes.  
  Example: `null`

- **phone** *str | null*  
  Phone number.  
  Example: `null`

- **email** *str | null*  
  Email address.  
  Example: `null`

- **postcode** *str | null*  
  Postal code.  
  Example: `null`

- **city** *str | null*  
  City.  
  Example: `null`

- **state** *str | null*  
  State.  
  Example: `null`

- **address** *str | null*  
  Street address.  
  Example: `null`

- **created_at** *str*  
  Creation timestamp.  
  Example: `2025-05-11T19:51:32.811784-03:00`

- **updated_at** *str*  
  Last update timestamp.  
  Example: `2025-05-11`

</details><br>


# Stack

### Backend

- Python + Django

### Infrastructure

- **AWS**
  - EC2 (hosting)
  - RDS (PostgreSQL)
  - Route 53 (DNS)
- **Nginx** as reverse proxy
- **Celery** for updating payment status
- **RabbitMQ** for monitoring payment due dates
- **GitHub Actions** for CI/CD
