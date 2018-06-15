# wallet

## base_url

```
http://52.15.127.40
```

## auth

### /signup
```
type: POST

request body:
{
    email: required,
    password: required,
    first_name: required,
    last_name: required,
    phone_number: optional
    phone_number_extension: optional
}

response:
{
    data: JWT_TOKEN
}
```

### /login
```
type: POST

request body:
{
    email: required,
    password: required
}

response:
{
    data: JWT_TOKEN
}
```

### /reset_password (get reset code)
```
type: GET

request body:
{
    email: required
}
```

### /reset_password
```
type: POST

request body:
{
    code: required,
  password: required
}
```

## users

### /users (create user under account)
```
type: POST

request body:
{
    email: required,
    password: required,
    first_name: required,
    last_name: required,
    status: optional
    phone_number: optional
    phone_number_extension: optional
}
```

### /users/:id (update user)
```
type: PUT

request body:
{
    email: optional,
    first_name: optional,
    last_name: optional,
    status: optional
    phone_number: optional
    phone_number_extension: optional
}
```

### /users/changePassword/:id
```
type: PUT

request body:
{
    password: required
}
```

## users

### /accounts/:id (update account)
```
type: PUT

request body:
{
    email_account: optional,
    company_name: optional,
    industry: optional,
    time_zone: optional,
    locale: optional,
    website: optional,
    address: optional,
    city: optional,
    state: optional,
    zipcode: optional,
    country: optional
}
```
