# Authentication

Zola uses an API key and token to authenticate users. 

`Want to start playing around?` 
`Shoot us an email and we'll set you up with a test key.` 

## Client Authentication

All client's API Requests require a signature Key and an API Key. 

|Parameter | Description
|--------- | ------- |
|*signature* | A hash of a token. Contact Zola for more information. |
|*key* | A client-specific key. |

## User Authentication

Any interactions on behalf of a user require three sets of data: A member_id, an auth_token, and a device_name. member_id and auth_token can be retrieved using the /session API.

