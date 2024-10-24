---
title: Polaris Docs
permalink: /docs/home/
redirect_from: /docs/index.html
---

## Types of request classes

Most Polaris API endpoints take one of the two defined class arguments. These arguments should be passed as strings, even if you are giving a number. We do this purely for convenience, as it's easier to remember to just `tostring` or `str()` something for example.

Here are the two types of requests we accept:

### RequestUser

```json
{
  "owner_secret": "your_owner_secret",
  "gameid": "123456",
  "userid": "123456"
}
```

### BanRequest

```json
{
  "owner_secret": "your_owner_secret",
  "gameid": "123456",
  "userid": "123456",  (userID to ban, not the moderator. if you want to check if that user is a moderator and THEN use this, use /auth/check_mod)
  "reason": "string"
  
}
```

### RequestNoUser

```json
{
  "owner_secret": "your_owner_secret",
  "gameid": "123456"
}
```

We will tell you which endpoints use which, so please refer back to this or keep it in your mind while you read the documentation.

---

## Request types

The following endpoints use the following methods:

**GET**

- /status
- /version

**POST**
- /misc/motd
- /auth/check_game
- /auth/creds
- /auth/check_mod
- /auth/mods
- /auth/create_mod
- /auth/remove_mod
- /mod/ban
- /mod/unban
- /mod/bans


---

## Authentication

In this section, we will be showing you authentication endpoints. These have functionality like checking if a ID is a moderator, adding moderators, etc.

**Please note that the "message" arguments in some responses are meant to be displayed to your members, outputted to logs, or exfiltrated to your webhooks. Some responses might not have these, so please perform a check in your code to see if the response had a message argument. If you think some aren't long enough or don't fit right, you can implement logic to check if the status was `true` or `false`, and return your own strings.**

### /auth/create_mod

`Request Type: Request User` - `Method: POST`
This endpoint will create a moderator for your game. Please note that you **really need to practice good security** checks on the server administering requests for this. Use this with the combination of the **/auth/creds (don't let mods add more mods)** endpoint to make sure that the user performing the action is whitelisted (only supports Roblox/Discord ID's, if you would like more, post a suggestion)

To fufill the request, please pass the required JSON data with your request. Here is an example written in Python:

```python
import requests

data = {
  "owner_secret": "your_owner_secret",
  "gameid": "123456",
  "userid": "123456"
}

request = requests.post("https://api.polarisadmin.xyz/auth/create_mod", data=data)
print(request.json())
```

Make sure that all requests are converted to a dictionary (decoding), however, if you are using our HTTPS module provided (assuming you're looking for documentation), you can just use our provided HTTPS module or copy the general idea of that and port it to Python.

The API server will send back a response with the provided data:

```json
{
  "status": "true",
  "message": "User banned"
}
```

If the request was missing a required arguments, then it will return something like this:

```json
{
  "status": "false",
  "message": "Invalid owner secret, contact your game owners immediately."
}
```

### /auth/remove_mod
`Request Type: UserRequest`  `Method: POST`
This endpoint removes a moderator. As pointed out in **/auth/create_mod**, you **must** practice atleast the barebones in terms of safety, by referring every request from the client to your server (which acts as the boundary between `client -> api`), and make the server itself communicate to the API, checking if that user is authorized.

Since this endpoint is the UserRequest type, we can go and send an example request:

```python
import requests

data = {
  "owner_secret": "your_owner_secret",
  "gameid": "123456",
  "userid": "123456"
}

request = requests.post("https://api.polarisadmin.xyz/auth/remove_mod", data=data)
print(request.json())
```

When we run the following, we will receive a response. A good response always returns a dictionary that contains the "status" string.

Remember, all responses are given in strings, regardless of input. That being said, **your server MUST convert values to strings before passing them to the server**.

Successful response:
```json
{
    "status": "true",
    "message": "The moderator was removed"
}
```
Unsuccessful response:
```json
{
    "status": "false",
    "message": "Invalid owner secret, contact your game owners immediately."
}
```

### /auth/check_game

`Request Type: RequestNoUser` - `Method: POST` This endpoint checks if the corresponding `gameid` and `owner_secret` in the following strings are valid/is an actual registered and game.

```json
{
  "owner_secret": "string",
  "gameid": "string"
}
```

The response will look like this:

```json
{
  "status": "true",
  "check": "true"
}
```

If you are missing an argument, it will look like this:

```json
{
  "status": "false",
  "message": "This owner secret is not valid, please contact your game owner."
}
```

### /auth/mods

`Request Type: RequestNoUser` - `Method: POST` This endpoint checks the list of moderators of the corresponding `gameid` and `owner_secret` listed.

```json
{
  "owner_secret": "string",
  "gameid": "string"
}
```

The response will look like this:

```json
{
  "status": "true",
  "moderators": [
    "num1",
    "num2",
    "cont",
  ]
}
```

If you are missing an argument, it will look like this:

```json
{
  "status": "false",
  "message": "This owner secret is not valid, please contact your game owner."
}
```

### /auth/check_mod

`Request Type: Request User` - `Method: POST`

This endpoint checks if a specified UserID is a moderator. The server will perform a check in realtime, making it so that you can instantly remove one of your moderators; and it will update cross-platform with no delay.

To fufill the request, you need to send all of the arguments. Check out the example below:

```json
{
  "owner_secret": "your_owner_secret",
  "gameid": "123456",
  "userid": "123456"
}
```

The response will look like this:

```json
{
  "status": "true",
  "is_admin": "true"
}

{
  "status": "true",
  "is_admin": "false"
}
```

If you are missing an argument, it will look like this:

```json
{
  "status": "false",
  "message": "Invalid owner secret, contact your game owners immediately."
}
```

### /auth/creds

`Request Type: RequestNoUser` - `Method: POST` This endpoint checks the roblox UserId and Discord user Id of the person that has purchased the key of the corresponding `gameid` and `owner_secret` in the following strings.

```json
{
  "owner_secret": "string",
  "gameid": "string"
}
```

The response will look like this:

```json
{
  "status": "true",
  "roblox": "num",
  "discord": "num"
}
```

If you are missing an argument, it will look like this:

```json
{
  "status": "false",
  "message": "This owner secret is not valid, please contact your game owner."
}
```

