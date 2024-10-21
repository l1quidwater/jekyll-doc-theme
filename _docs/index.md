---
title: Polaris Docs
permalink: /docs/home/
redirect_from: /docs/index.html
---
___
## Types of request classes
Most Polaris API endpoints take one of the two defined class arguments. These arguments should be passed as strings, even if you are giving a number. We do this purely for convenience, as it's easier to remember to just ```tostring``` or ```str()``` something for example.

Here are the two types of requests we accept:
### RequestUser
```json
{
  "owner_secret": "your_owner_secret",
  "gameid": "123456",
  "userid": "123456"
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
___
## Authentication
In this section, we will be showing you authentication endpoints. These have functionality like checking if a ID is a moderator, adding moderators, etc.

### /auth/create_mod
`Request Type: Request User`

### /auth/check_mod
`Request Type: Request User`


