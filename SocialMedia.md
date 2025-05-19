
# Overview

This API enables a social media application with features for user management, social connections and content interaction. It supports user sign up, log in, sending and managing friend requests, creating, liking and listing posts. This API follows the standard RESTful conventions, and prioritises security. It is desigend for scalaibility and ease of use for developers. 



# Base URL

The base URL for accessing all the endpoints is 

`https://api.socialapp.com/v1`



# Authentication

The API uses JWT (JSON Web token) for authentication. After a successful login or signup a token is returned top the client and must be used for every subsequent request, so that the user can be authenticated without them having to send their credentials every time. 

---

# Endpoints

## 1. User Sign Up

**Description:** Registers a new user with email, username and password. 

**URL:** `/auth/signup`

**Method:** `POST`

**Request Body:**

json
```
{
    "username": "MyName",
    "email": "myname@email.com",  
    "password": "MYPass123"
} 
```

**Response Body**
 
* On successful Signup (201 created)

    json
    ```
    {
    "message": "User Created Successfully",
    "user": {
        "id": "123abf54",
        "email": "myname@email.com",
        "username": "MyName",
        created_at: "Date Time"
        }
        "acsess_token": "hjbsdbhjsdhbs..,"
        "refresh_token": "xcmndnAw...."
    }
    ```

* On failure (400 Bad Request)

    json
    ```
    {
    "error": "Email already exists"
    }
    ```
Failure may also occur due to Internal Server Error (Status code 500).

**Validation Rules:**

* *email:* Valid email format, max 255 characters, must be unique. 

* *username:* Alphanumeric, min 5 characters, max 25 characters, must be unique. 

* *password:* Alphanumeric, min 8 characters, max 32 characters, must inlcude atleast one number, special character, uppercase and lowercase alphabet.


## 2. User Log In

**Description:** Authenticates a user and returns access and refresh tokens. 

**URL:** `/auth/login`

**Method:** `POST`

**Request Body:**

json
```
{
    "email": "myname@email.com",  
    "password": "MYPass123"
} 
```

**Response Body**
 
* On successful LogIn (201 OK)

    json
    ```
    {
    "message": "LogIn Successfull",
    "user": {
        "id": "123abf54",
        "email": "myname@email.com",
        "username": "MyName",
        }
        "acsess_token": "hjbsdbhjsdhbs..,"
        "refresh_token": "xcmndnAw...."
    }
    ```

* On failure (401 Unauthorized)

    json
    ```
    {
    "error": "Invalid Username or Password"
    }
    ```
Failure may also occur due to Internal Server Error (Status code 500) or Invalid Input(Status code 400).

**Validation Rules:**

* *email:* Required, Valid email format

* *password:* Required, 8-32 characters.


## 3. Send friend Request

**Description:** Sends friend request to another user. 

**URL:** `/friends/requests`

**Method:** `POST`

**Request Body:**

json
```
{
    "recipient_id": "8972fd62",  
} 
```

**Response Body**
 
* On success (201 Created)

    json
    ```
    {
    "message": "Friend Request Sent",
    "request": {
        "id": "167acf74",
        "sender_id": "1172df42",
        "recipient_id": "8972fd62",
        "status": "pending",
        "created_at": "Date Time"
        }
    }
    ```

* On failure (404 Unauthorized)

    json
    ```
    {
    "error": "No such recipient exists"
    }
    ```
Failure may also occur due to Internal Server Error (Status code 500).

**Validation Rules:**

* *recipient_id:* Must be a valid existing User ID and not the same as sender's id.

## 4. Accept or Reject friend Request

**Description:** Accepts or rejects a pending friend request from another user. 

**URL:** `/friends/respond/`

**Method:** `POST`

**Request Body:**

json
```
{
    "request_id": "167acf74"
    "action": "string (accept/reject)",  
} 
```

**Response Body**
 
* On success (200 OK)

    json
    ```
    {
    "message": "Friend Request Accepted/Rejected",
    "request": {
        "id": "167acf74",
        "status": "accpted/rejected",
        }
    }
    ```

* On failure (400 Bad Request)

    json
    ```
    {
    "error": "No such request exists"
    }
    ```
Failure may also occur due to Internal Server Error (Status code 500).

**Validation Rules:**

* *request_id:* Must be a valid request id for a request that has come to the responder and is still pending.

* *action:* Must be "accept" or "reject".



## 5. Create a Post

**Description:** Creates a new post. 

**URL:** `/posts`

**Method:** `POST`

**Request Body:**

json
```
{
    "content": "Good Morning everyone!!!"
} 
```

**Response Body**
 
* On success (201 Created)

    json
    ```
    {
    "message": "Post Created Successfully",
    "post": {
        "post_id": "167acf74",
        "user_id": ""123ba972"
        "content": "Good Morning everyone!!!",
        "created_at": "Date Time",
        "likes_count": 98,
        }
    }
    ```

* On failure (400 Bad Request)

    json
    ```
    {
    "error": "Charcter limit exceeded"
    }
    ```
Failure may also occur due to Internal Server Error (Status code 500).

**Validation Rules:**

* *content:* Must be within the charcater limit of 1024 characters. 

## 6. Like a Post

**Description:** Likes or Unlikes (toggles) a post. 

**URL:** `/posts/:post_id/likes/`

**Method:** `POST`


**Response Body**
 
* On success (200 OK)

    json
    ```
    {
    "message": "Post Liked/Unliked",
    "post": {
        "post_id": "167acf74",
        "liked": 99,
        }
    }
    ```

* On failure (404 Not found)

    json
    ```
    {
    "error": "Post Not Found!!!"
    }
    ```
Failure may also occur due to Internal Server Error (Status code 500).

**Validation Rules:**

* *post_id:* Must be valid id for a post that does exist.


## 7. List Posts

**Description:** Retrives a list of posts from user and/or their friends. 

**URL:** `/posts/:post_id/likes/`

**Method:** `GET`


**Response Body**
 
* On success (200 OK)

    json
    ```
    {
    "message": "Posts Retrived Successfully",
    "posts": {
        ... Post 1....
        },
        {
            ... Post 2 ...
        }

    }
    ```

* On failure (401 Unauthorized)

    json
    ```
    {
    "error": "You are not authorized to access these posts!!!"
    }
    ```
Failure may also occur due to Internal Server Error (Status code 500).

---

# Validation Summary

| Field | Rules |
|--------|--------|
| email | Valid email format, max 255 characters |
| username | Alphanumeric, 5-25 characters, unique |
| password | Alphanumeric, 8-32 characters, must inlcude atleast one number, special character, uppercase and lowercase alphabet. |
| recipient_id | Valid id for a user other than self. |
| request_id | Valid id for a request sent to this user. |
| post_id | Valid id for a post that exists. |
| action | `accept` or `reject` |
| content | Max 1024 characters |



# Security Best Practices

* Uses HTTPS for all requests.
* Used strong hashing bcrypt to protect passwords.
* Strict Input Validation to prevent attacks like SQL, XSS etc.
* JWT authentication helps enhance security with help of access and refresh tokens. 
