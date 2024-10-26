## Library Management System

### Prerequisites
1. Make Sure you have docker,k8s,python Installed in your local system or virtual machine and all the configuration has been made up of.

![Sample Overwiew](Images/Img2.PNG)

# Technical Stack

- **users-service**:

  - REST API
  - Python
  - FastAPI
  - **user-db**:
    - Postgres

- **books-service**:

  - REST API
  - Python
  - FastAPI
  - **user-db**:
    - Postgres

- **library-frontend**:
  - React JS

#### To run book database

- image: postgres:15-alpine
- environmental variables:
    - POSTGRES_USER: user
    - POSTGRES_PASSWORD: password
    - POSTGRES_DB: booksdb
- port: 5432

#### To run book Service

- image: shaikkhajaibrahim/libbookssvc:1.0
- environmental variables:
  - DATABASE_URL: “postgresql://:@:5432/“
  - SECRET_KEY: ‘YtDEVWnL35aAIP-5yxeLjAZ49R920-mMNDfwPyWULu63HFsYzo0f-LO2InxC8eu428k’
- port: 8000

#### To run user database

- image: postgres:15-alpine
- environmental variables:
  - POSTGRES_USER: user
  - POSTGRES_PASSWORD: password
  - POSTGRES_DB: booksdb
- port: 5432

#### To run User Service

- image: shaikkhajaibrahim/libbookssvc:1.0
- environmental variables:
  - DATABASE_URL: “postgresql://:@:5432/“
  - SECRET_KEY: ‘YtDEVWnL35aAIP-5yxeLjAZ49R920-mMNDfwPyWULu63HFsYzo0f-LO2InxC8eu428k’
- port: 8000

#### To run library web-store

- image: shaikkhajaibrahim/libwebstore:1.0
- environmental variables
  - REACT_APP_BACKEND_API_URL: http://:8000/api/v1
  - REACT_APP_BOOKS_API_URL: http://:8000/api/v1/books
  - REACT_APP_USERS_API_URL: http://:8000/api/v1/users
- port: 3000
