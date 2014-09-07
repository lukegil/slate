# User Accounts

This endpoint allows you to create accounts, log users in, request authentication tokens, retrieve profile information, and update user passwords. 

Most endpoints require both client-level and user-level authentication. 

## Create Account

> To create an account:

```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"action": "create",
		"email": "test@test.com",
		"password": "testing123",
		"member_first_name": "Test",
		"bday_d": "1",
		"bday_m": "1",
		"bday_y": "1980",
		"device_name": "Device Name",
		"member_last_name": "Last",
		"member_gender": "n/a|m|f",
	}'
	https://api.zo.la/v4/session/account
```

> If successful, JSON of the following form is returned:

```json
{ 
	"status" : "success",
	"data": {
           "member_id": "unique-identifier",
           "auth_token": "7ef4211c325174440c782ec5ba3c2400",
           "device_name": "device",
           "user" : {
                 "username": "unique-name",
                 "screen_name": "Full Name",
                 "email": "test@test.com",
                 "bday_d": "1",
                 "bday_m": "1",
                 "bday_y": "1980",
                 "birthdate": "1980-01-01",
                 "avatar_url": "url.com/image.jpg",
                 "first_name": "Luke",
                 "last_name": "Gilson",
                 "facebook_username": "luke.gilson",
                 "twitter_username": "luke.gilson",
                 "gender": "m",
                 "zip_code": "10018",
                 "location": "new york",
                 "public_email": "luke.gilson@zolabooks.com",
                 "website": "http://mysite.com/",
                 "twitter_url": "luke.gilson",
                 "facebook_url": "luke.gilson",
                 "phone": "123-456-7890",
                 "gplus_url": "luke.gilson",
                 "receive_newsletter": "n",
                 "facebook_id": "luke.gilson",
                 "gplus_username": "luke.gilson",
                 "receive_email_if_followed": "n",
                 "receive_email_if_messaged": "y"
            }
}
```

Accounts can only be created for users older than 13 years. 

The most important information, from an API perspective, is the member_id, auth_token, and device_name, as these allow use of any other user-specific apis (e.g. purchase, update password).

### HTTP Request

`POST http://api.zo.la/v4/session/account`


### Post Parameters

|Parameter | Type | Description
|--------- | ------- | ------- |
| *action* | **create** | Create an account for a user. |
| *email* | string | _Required_ <br> The email account for a user. |
| *password* | string | _Required_ <br> The password a user wishes to use. |
| *member_first_name* | string | _Required_ <br> The first name of the user. |
| *bday_d* | integer | _Required_ <br> The day portion of the birthday of the user. |
| *bday_m* | integer | _Required_ <br> The numeric month of the user's birthday. e.g. May would be '5'. |
| *bday_y* | integer | _Required_ <br> The four digit, user's birth-year. |
| *device_name* | string | _Required_ <br> The name of the device the user is on. This can be the name of their iPad or iPhone, or simply their browser information. |
| *member_last_name* | string | _Optional_ <br> The last name of the user. |
| *member_gender* | string | _Optional_ <br> _Default_: n/a <br> The gender of the user, either m | f | n/a.


## Login

```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"email": "test@test.com" | "username" : "unique-user-name",
		"password" : "password",
		"device_name" : "Device
	}'
	https://api.zo.la/v4/session/login
```	

> If successful, JSON of the following form is returned:

```json
{ 
	"status" : "success",
	"data": {
           "member_id": "unique-identifier",
           "auth_token": "7ef4211c325174440c782ec5ba3c2400",
           "device_name": "device",
           "user" : {
                 "username": "unique-name",
                 "screen_name": "Full Name",
                 "email": "test@test.com",
                 "bday_d": "1",
                 "bday_m": "1",
                 "bday_y": "1980",
                 "birthdate": "1980-01-01",
                 "avatar_url": "url.com/image.jpg",
                 "first_name": "Luke",
                 "last_name": "Gilson",
                 "facebook_username": "luke.gilson",
                 "twitter_username": "luke.gilson",
                 "gender": "m",
                 "zip_code": "10018",
                 "location": "new york",
                 "public_email": "luke.gilson@zolabooks.com",
                 "website": "http://mysite.com/",
                 "twitter_url": "luke.gilson",
                 "facebook_url": "luke.gilson",
                 "phone": "123-456-7890",
                 "gplus_url": "luke.gilson",
                 "receive_newsletter": "n",
                 "facebook_id": "luke.gilson",
                 "gplus_username": "luke.gilson",
                 "receive_email_if_followed": "n",
                 "receive_email_if_messaged": "y"
            }
}
```
Logs a user into their account by returning authentication information.

### HTTP Request

`POST http://api.zo.la/v4/session/login`


### Post Parameters

|Parameter | Type | Description
|--------- | ------- | ------- |
| *email* | string | _Optional_ <br> The email account for a user. Username required if email not provided. |
| *username* | string | _Optional_ <br> A hyphen-separated unique name for a user. Email required if username not provided. |
| *password* | string | _Required_ <br> A user's password |
| *device_name* | string | _Required_ <br> The name of the device the user is on. This can be the name of their iPad or iPhone, or simply their browser information. |


## Login with Social Network

```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"token": "$Oauth-token",
		"type": "facebook|twitter|gplus|goodreads",
		"device_name": "device",
		"email": "george.gilson@zolabooks.com"
	}'
	https://api.zo.la/v4/session/login_social
```

> If successful, JSON of the following form is returned:

```json
{ 
	"status" : "success",
	"data": {
           "member_id": "unique-identifier",
           "auth_token": "7ef4211c325174440c782ec5ba3c2400",
           "device_name": "device",
           "user" : {
                 "username": "unique-name",
                 "screen_name": "Full Name",
                 "email": "test@test.com",
                 "bday_d": "1",
                 "bday_m": "1",
                 "bday_y": "1980",
                 "birthdate": "1980-01-01",
                 "avatar_url": "url.com/image.jpg",
                 "first_name": "Luke",
                 "last_name": "Gilson",
                 "facebook_username": "luke.gilson",
                 "twitter_username": "luke.gilson",
                 "gender": "m",
                 "zip_code": "10018",
                 "location": "new york",
                 "public_email": "luke.gilson@zolabooks.com",
                 "website": "http://mysite.com/",
                 "twitter_url": "luke.gilson",
                 "facebook_url": "luke.gilson",
                 "phone": "123-456-7890",
                 "gplus_url": "luke.gilson",
                 "receive_newsletter": "n",
                 "facebook_id": "luke.gilson",
                 "gplus_username": "luke.gilson",
                 "receive_email_if_followed": "n",
                 "receive_email_if_messaged": "y"
            }
}
```
This allows login and account creation using Facebook, Twitter, Goodreads, or Google Plus. `token` is the token or signature key returned by the service's OAuth.

### HTTP Request

`POST http://api.zo.la/v4/session/login_social`


### Post Parameters

|Parameter | Type | Description
|--------- | ------- | ------- |
| *type* | string | _Required_ <br> The name of the social network being used. Either "facebook", "twitter", "gplus", or "goodreads".
| *email* | string | _Optional_ <br> If the social service returns an email address, that will be used. If no email is returned by the social service, this is required. If an email is returned, and this is provided, this will be used. |
| *token* | string | _Required_ <br> This token is generated by the oAuth access_token method. |
| *device_name* | string | _Required_ <br> The name of the device the user is on. This can be the name of their iPad or iPhone, or simply their browser information. |


## Is Logged In

```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"auth_member_id": "unique-id",
		"auth_token": "$token"
	}'
	https://api.zo.la/v4/session/is_logged_in
```

> If successful, returns the following JSON structure:

```json
{
	"status": "success",
	"data": {
    	"member_id": 78395,
    	"auth_token": "7ef4211c325174440c782ec5ba3c2400",
   		"device_name": "device"
   	}
}
```

Returns if a user is logged in or not.

### HTTP Request

`POST http://api.zo.la/v4/session/is_logged_in`


### Post Parameters

|Parameter | Type | Description
|--------- | ------- | ------- |
| *auth_member_id* | string | _Required_ <br> The unique identifier for a user. |
| *auth_token* | string | _Required_ <br> The authentication token for a user. |


## Logout

```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"auth_member_id": "unique-id",
		"auth_token": "$token",
		"all" : true | false
	}'
	https://api.zo.la/v4/session/is_logged_in
```

> If successful, returns a 204. 

This will log a user out of either the specified device or all devices a user is logged in on. 

### HTTP Request

`POST http://api.zo.la/v4/session/logout`


### Post Parameters

|Parameter | Type | Description
|--------- | ------- | ------- |
| *auth_token* | string | _Required_ <br> The authentication token for a user. |
| *auth_member_id* | string | _Required_ <br> The unique identifier for a user. |
| *all* | logical | _Optional_ <br> If true, will log a user our of all devices. Otherwise, specify device_name.


## Change Password

```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"auth_member_id": "unique-id",
		"auth_token": "$token",
		"action" : "update",
		"username" : "unique-name",
		"email" : "test@test.com",
		"current_password" : "password",
		"password" : "new password"
	}'
	https://api.zo.la/v4/session/password
```

> If successful, returns the following JSON:

```json
{ 
	"status" : "success",
	"data": "password updated"
}
```

This allows a client to change a users password. 

### HTTP Request

`POST http://api.zo.la/v4/session/password`


### Post Parameters

|Parameter | Type | Description
|--------- | ------- | ------- |
| *auth_token* | string | _Required_ <br> The authentication token for a user. |
| *auth_member_id* | string | _Required_ <br> The unique identifier for a user. |
| *action* | **update** | _Required_
| *email* | string | _Optional_ <br> If not provided, must provide username. |
| *username* | string | _Optional_ <br> If not provided, must provide email. |
| *current_password* | string | _Required_ <br> A user's current password. |
| *password* | string| _Required_ <br> The password a user would like to update to. |


## Forgot Password

```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"auth_member_id": "unique-id",
		"auth_token": "$token",
		"action" : "forgot",
		"email" : "test@test.com",
	}'
	https://api.zo.la/v4/session/password
```

> If successful, returns the following JSON:

```json
{ 
	"status" : "success",
	"data": "email sent"
}
```

This will send a user an email, with a link to reset their password.

### HTTP Request

`POST http://api.zo.la/v4/session/password`


### Post Parameters

|Parameter | Type | Description
|--------- | ------- | ------- |
| *action* | **forgot** | _Required_ |
| *email* | string | _Optional_ <br> If not provided, must provide username. |


## Auth Tokens

```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"auth_member_id": 78395,
		"auth_token": "7ef4211c325174440c782ec5ba3c2400",
		"action": "deactivate" | "regenerate",
	}'
	https://api.zo.la/v4/session/token
```

> If action = regenerate, the following JSON is returned:

```json
{ 
	"status" : "success",
    "data": {
        "member_id": 78395,
        "auth_token": "7ef4211c325174440c782ec5ba3c2400",
        "device_name": "device"
    }
}
```

> If action = deactivate, client receives a 200.

This allows clients to regenerate user authentication tokens. 

### HTTP Request

`POST http://api.zo.la/v4/session/token`


### Post Parameters

|Parameter | Type | Description
|--------- | ------- | ------- |
| *auth_token* | string | _Required_ <br> The authentication token for a user. |
| *auth_member_id* | string | _Required_ <br> The unique identifier for a user. |
| *action* | string | _Required_ |
| | **deactivate** | |
| | **regenerate** | |



## Get Profile Info

```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"member_id": 12, 
		"auth_member_id": 78395,
		"auth_token": "7ef4211c325174440c782ec5ba3c2400",
		"action": "get | get-min"
	}'
	https://api.zo.la/v4/session/profile
```

> Returns JSON with the following structure:

```json
{ 
	"status" : "success",
	"data": {
           "member_id": "unique-identifier",
           "user" : {
                 "username": "unique-name",
                 "screen_name": "Full Name",
                 "email": "test@test.com",
                 "bday_d": "1",
                 "bday_m": "1",
                 "bday_y": "1980",
                 "birthdate": "1980-01-01",
                 "avatar_url": "url.com/image.jpg",
                 "first_name": "Luke",
                 "last_name": "Gilson",
                 "facebook_username": "luke.gilson",
                 "twitter_username": "luke.gilson",
                 "gender": "m",
                 "zip_code": "10018",
                 "location": "new york",
                 "public_email": "luke.gilson@zolabooks.com",
                 "website": "http://mysite.com/",
                 "twitter_url": "luke.gilson",
                 "facebook_url": "luke.gilson",
                 "phone": "123-456-7890",
                 "gplus_url": "luke.gilson",
                 "receive_newsletter": "n",
                 "facebook_id": "luke.gilson",
                 "gplus_username": "luke.gilson",
                 "receive_email_if_followed": "n",
                 "receive_email_if_messaged": "y"
            }
}
```

This endpoint is used to retrieve profile information for a user. 

### HTTP Request

`POST http://api.zo.la/v4/session/profile`

### Post Parameters

|Parameter | Type | Description
|--------- | ------- | ------- |
| *auth_token* | string | _Required_ <br> The authentication token for the user making the request. |
| *auth_member_id* | string | _Required_ <br> The unique identifier for the user making the request. |
| *member_id* | string | _Required_ <br> The unique identifier of the user information is being requested for. |
| *action* | string <br><br>**get** | _Required_ <br><br> Get all available information for a user. |
| | **get-min** | Get a subset of possible info |


## Update Profile Info

```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
		"member_id": 12, 
		"auth_member_id": 78395,
		"auth_token": "7ef4211c325174440c782ec5ba3c2400",
		"action": "set",
		"keys" : "values"
		"..."
	}'
	https://api.zo.la/v4/session/profile
```

> If the update is successful, the following JSON is returned:

```json
{ 
	"status" : "success",
	"data": "profile updated"
}
```


This is to update any field that can be retrieve in `Get Profile Info`. Only updated fields need to be submitted.

### HTTP Request

`POST http://api.zo.la/v4/session/profile`

### Post Parameters

|Parameter | Type | Description
|--------- | ------- | ------- |
| *auth_token* | string | _Required_ <br> The authentication token for the user making the request. |
| *auth_member_id* | string | _Required_ <br> The unique identifier for the user making the request. |
| *action* | string <br><br>**set** | _Required_ <br><br> Update the the following fields in user's profile. |
| *key* | *value* | _Optional_ <br> All fields are optional. They should be formatted in as valid JSON. See Get Profile Info, above. |


## avatar

```shell
Form:

curl -H "Content-Type: application/json" -d '
	{
	}'
	https://api.zo.la/v4/session/avatar
```
> Returns JSON with the following structure

```json
{
	"status": fail,
	"data": "invalid action"
}
```


## ping

```shell

Form:

curl -H "Content-Type: application/json" -d '
	{
	
	}'
	https://api.zo.la/v4/session/ping
```


