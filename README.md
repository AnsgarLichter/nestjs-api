# NEST API

An API to create posts for an user. This project is used to test the compatibility of Nest with [Bun](https://bun.sh/).
You will find the Node.js version in the [main](https://github.com/AnsgarLichter/nestjs-api) branch and the version for Bun in the branch [migrate-to-bun](https://github.com/AnsgarLichter/nestjs-api/tree/migrate-to-bun).

This repository is based on the [NestJS Starter Kit [v2]](https://github.com/monstar-lab-oss/nestjs-starter-rest-api).

## Features

This API includes the following features:

| Feature                  | Info               | Progress |
|--------------------------|--------------------|----------|
| Authentication           | JWT                | Done     |
| Authorization            | RBAC (Role based)  | Done     |
| ORM Integration          | TypeORM            | Done     |
| DB Migrations            | TypeORM            | Done     |
| Logging                  | winston            | Done     |
| Request Validation       | class-validator    | Done     |
| Pagination               | SQL offset & limit | Done     |

## Installation

Install the dependencies

```bash
bun install
```

Generate JWT public and private key pair for jwt authentication.

```bash
ssh-keygen -t rsa -b 2048 -m PEM -f jwtRS256.key
# Don't add passphrase
openssl rsa -in jwtRS256.key -pubout -outform PEM -out jwtRS256.key.pub
```

You may save these key files in `./local` directory as it is ignored in git.

Start an PostgreSQL server locally and create a new database:
[PostgreSQL Documentation](https://www.postgresql.org/docs/current/tutorial-start.html)

Create a .env file with the following properties:

```dotenv
JWT_PUBLIC_KEY_BASE64="<public key>"
APP_PORT=<port>
DB_HOST=<db host>
DB_PORT=<db port>
DB_NAME=<db name>
DB_USER=<db user>
DB_PASS=<db password>
JWT_ACCESS_TOKEN_EXP_IN_SEC=<seconds>
JWT_REFRESH_TOKEN_EXP_IN_SEC=<seconds>
DEFAULT_ADMIN_USER_PASSWORD=<password>

```



Encode keys to base64:

```bash
base64 -i local/jwtRS256.key

base64 -i local/jwtRS256.key.pub
```

## Running the app

The Postgres server must berunning for the app to work

```bash
# development
$ bun run start

# watch mode
$ bun run start:dev
```

The production mode isn't available with Bun because the bundler doesn't work with Nest at the moment. This may be fixed as soon as the issue [https://github.com/oven-sh/bun/issues/4803](https://github.com/oven-sh/bun/issues/4803) is solved.

## Sending API requests

### Registration & Login

#### Register

| Property | Value                  |
|----------|------------------------|
| Endpoint | /api/v1/auth/register  |
| Method   | POST                   |
| Body     | User                   |
| Response | Registered User        |

```json
{
    "name": "<full name>",
    "username": "<username>",
    "password": "<password>",
    "roles": "<roles: admin / user>",
    "email": "<email>",
    "isAccountDisabled": <boolean>
}
```

#### Login

| Property | Value                  |
|----------|------------------------|
| Endpoint | /api/v1/auth/login     |
| Method   | POST                   |
| Body     | Username & Password    |
| Response | Tokens                 |

```json
{
    "username": "<username>",
    "password": "<password>"
}
```

#### Refresh Token

| Property | Value                      |
|----------|----------------------------|
| Endpoint | /api/v1/auth/refresh-token |
| Method   | POST                       |
| Body     | Refresh Token              |
| Response | Tokens                     |

```json
{
    "refreshToken": "<token>"
}
```

### Users

You have to supply a valid Bearer Token for the authorization check to work.
If you don't have an user, please register first.

#### Read your own Profile

| Property | Value            |
|----------|------------------|
| Endpoint | /api/v1/users/me |
| Method   | GET              |
| Body     | -                |
| Response | User             |

#### Read all Users

| Property | Value            |
|----------|------------------|
| Endpoint | /api/v1/users    |
| Method   | GET              |
| Body     | -                |
| Response | All Users        |

#### Read an User

| Property | Value              |
|----------|--------------------|
| Endpoint | /api/v1/users/{id} |
| Method   | GET                |
| Body     | -                  |
| Response | User               |

#### Update an User

| Property | Value                  |
|----------|------------------------|
| Endpoint | /api/v1/articles/{id}  |
| Method   | PATCH                  |
| Body     | Username & Password    |
| Response | User                   |

```json
{
    "name": "<name>",
    "password": "<password>"
}
```

### Articles

You have to supply a valid Bearer Token for the authorization check to work.
If you don't have an user, please register first.

#### Creating an Article

| Property | Value            |
|----------|------------------|
| Endpoint | /api/v1/articles |
| Method   | POST             |
| Body     | Article          |
| Response | Created Article  |

```json
{
    "title": "<title>",
    "post": "<post>"
}
```

#### Read all Articles

| Property | Value            |
|----------|------------------|
| Endpoint | /api/v1/articles |
| Method   | GET              |
| Body     | -                |
| Response | All Articles     |

#### Read an Article

| Property | Value                 |
|----------|-----------------------|
| Endpoint | /api/v1/articles/{id} |
| Method   | GET                   |
| Body     | -                     |
| Response | Article               |

#### Update an Article

| Property | Value                  |
|----------|------------------------|
| Endpoint | /api/v1/articles/{id}  |
| Method   | PATCH                  |
| Body     | Article                |
| Response | Updated Book           |

```json
{
    "title": "<title>",
    "post": "<post>"
}
```

#### Delete an Article

| Property | Value                 |
|----------|-----------------------|
| Endpoint | /api/v1/articles/{id} |
| Method   | DELETE                |
| Body     | -                     |
| Response | -                     |
