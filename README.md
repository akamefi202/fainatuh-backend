

Configuration file app `.sample-env` rename to `.env` with all details filled as required.

Runs the app in the development mode.

`npm run dev`

Runs the app in the production mode.

`npm run start`

Runs the app from Docker

`sudo docker-compose up`

Host app `http://localhost:3003`

AdminMongo `http://0.0.0.0:1234` (Mongo uri in docker containers `mongodb://mongo`)

Mongo (local) `mongodb://localhost:27018`

## Method API

### Get token 

`POST: /api/auth/signin`

Params

`
email:test@mail.com
password:test
`

### Register new user

`POST: api/auth/signup`

Params

`
email:test@mail.com
name:test
`

Password auto generate, find in local email to `/api-server/logs/mail`

### Refresh token

`POST: api/auth/refresh-tokens`

Params

`
refreshToken:idtoken::token
`

### Restore password

`POST: api/auth/restore-password`

Params

`
email:test@mail.com
`

### Confirm restore password

`POST: api/auth/confirm-restore-password`

Params

`
token:token
`

Token find in local email to `/api-server/logs/mail`

### Logout (with Authorization: Bearer token)

`POST: api/auth/logout`

### Exemple url Users List (with Authorization: Bearer token)

`GET: api/users`

## Error and logging

`/api-server/logs` Write logs error\combined log to prod mode

`/api-server/logs/mail` Dev mode catch send email and save .eml file

For logging use `winston` to `/api-server/src/utils/logger.js`

Http request console log `morgan`

Email sending errors are also additionally logged with the error type `EMAIL_ERROR`.

Handle error to `/api-server/src/middleware/ErrorHandler.js`

### Error validation

For validate data use AJV (https://github.com/epoberezkin/ajv)

Schemas validation to `/api-server/src/schemas`

Example validation route `router.post("/auth/signin", Validate.prepare(authSchemas.signin)`

Example validation error (http code 422): 

```js
{
    "code": 422,
    "message": "Unprocessable Entity",
    "errorValidation": {
        "fields": {
            "email": [
                "Should have required property 'email'"
            ],
            "password": [
                "Should have required property 'password'"
            ]
        }
    }
}
```

## Extra configuration

# Recaptcha
adding a new configureation for google Recaptcha v3 
GOOGLE_RECAPTCHA_SECRET_KEY

# AWS
Add a new configuration for AWS S3
AWS_S3_BUCKET
AWS_ACCESS_KEY
AWS_SECRET_KEY