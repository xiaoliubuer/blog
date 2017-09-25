---
layout: post
title:  "RESTful API"
date:   2017-08-31 11:51:11 -0400
categories: articles
---


URL: http://www.adamyang.com/article
```
URL  =  HTTP   +   hostname       +    URI
e.g.   http://   www.adamyang.com   /articles
```

#### Two URIs per resource:

collection: /contacts

item: 		/contacts/:id

# Hypertext
Hyperlinks link to documents 
Resource Locaitions

HTTP exchange 

Client   <--Data--  Web services


Action based


Resource based URL
zipcode/12345

GET POST PUT DELETE

Metadata


HTTP Status code
200 Success
500 Server error
404 Not found

Header Metadata

Contex
# Enssital for a RESTful api
Resource based URIs
HTTP methods
HTTP status codes
Message headers


# How to design RESTful api?
Design is more important then implement


# Resource based URL

#### level 1

* Instance Resource URIs

They have the unique id in the URI to identify what the intance they interested.

/messages/{messageID}
/profiles/{profileName}

Comment

Resource relations
Messages 1----m comments

/message/comment


/messages/1/comments/2
/message/{messageId}/comments/{commentId}


/messages/{messageId}/shares/{shareId}

/profiles/{profileName}/messages{messageId}

* Collection Resource URIs

What if want to get all message
/messages

What if want to get all comments of message 2?
/messages/2/comments

/messages/{messageId}/comments

/messages/{messageId}/likes
/messages/{messageId}/shares


#### Filtering results

parameters
/messages?offest=30&limit=10

offset: starting point
limit: page size

/messages?year=2014&offset=30&limit=10

* Two types of resource URIs
	* Instance resource URIs
	* Collection resource URIs
* Query parameters for pagination and filtering colleciton


# HTTP Methods

URIs
/getMessages.do?id=10 
This URI is action based

getMessages: action based


/message/10
resource based

HTTP Methods
GET POST PUT DELETE

HEAD OPTIONS

When type into the browser, I wil use the get method.

/getProucts.do?id=10  NO!!

GET  /products/10 	Yes!

/deleteOrder.do?id=10 NO!!

DELETE /orders/10 	Yes!


Messenger

Getting a message
GET /messages/{messageId}

Example:

Client  ---GET--->	/messages/20


Updateing a message
PUT /messages/{messageId}   Why not POST???


Example:

Client  ---PUT--->  /messages/20

Deleting a message

DELETE /messages/{messageId}

Example ---DELETE---> /messages/20


Creating a new message

POST 
post body should contain the content of the message

/messages
/profiles
/resources
/comments

Example:

Client ---POST---> /messages/21

Collection URL scenarios

GET /messages
gets "all" messages

DELETE /messages/10/comments
deletes "all" comments of message 10


GET /messages/10/comments

sets "all" comments of message 10



POST /message/10/comments

creates anew comment for message 10

PUT /messages/20/comments
replace "all" comments for message 20 with a new list


# idempotence
Idempotence is the property of certain operations in mathematics and computer science, that can be applied multiple times without changing the result beyond the initial appliction.

#### Differnce between PUT and POST

PUT: Idempotent
POST: Non-idempotent

Common
Read-only method: 	GET 

Write methods:		POST PUT DELETE

Get is safe to send a request to the server because nothing changed in the server.

#### Repeatable and Non-repeatable

Repeatable(GET, PUT, DELETE), safely repeatable(idempotent) and non-Repeatable(POST) operations, is non-idempotent mehtod.

Repatable: No matter how many times you repeat to sent this request, there's nothing change in server.

But non-repeatable, the server will be changed, for instance if you repeat to send POST request, it will continue to create a duplicate resources in server.

Caching GET Responses

Client ---GET---> /messages/20
Then next time to send the same GET request, we can retrive the result from cache.

Client safeguards


# REST Respones

Client <---Response--- Request URI
		(XML / JSON)


Java Class
```java
public class MessageEntity{
	int id;
	string message;
	Date created;
	string author;
}
```

JSON
```JSON
{
	"id": "10",
	"message": "Hello world",
	"created": "2014-06-01"
	"author": "adam"
}	
````

REST (Representational State Transfer)
Headers(Message length, Date, Content type )
Message Body

Status Codes
	200 OK
	404 Not Found

* 1XX Informational

* 2XX Success
	* 200 OK
	* 201 Created
	* 204 No Content

* 3XX Redirection
	* 302 Found
	* 307

* 4XX Client Error
	* 400 Bad Request
	* 401 Unauthorized
	* 403 Forbidden
	* 404 Not Found
	* 415 Unsupported Media Type

* 5XX Server Error
	* 500 Internal Server Error

# HATEOAS

No Service Definition
Hypertext links

Client <---Response--- Message Resource URL

{
	"id": "20",
	"message": "Hello world!",
	"author": "adam",
	"href": "/messages/1",
	"comments-href": "/messages/1/comments",
	"likes-href": "/messages/1/likes",
	"shares-href": "/messages/1/shares",
	"profile-href": "/messages/1/adam",
	"comment-post-href": "/message/1/comments"
}

The "rel" attribute

```html
<link rel="stylesheet" href="/css/bootstrap.css" />
<link rel="stylesheet" href="/css/font-awesome.css" />
```

Links with rel:
```JSON
{
	"id": "20",
	"message": "Hello world!",
	"author": "adam",
	"links": 
	[
		{
			"href": "/messages/1",
			"rel": "self"
		},
		{
			"comments-href": "/messages/1/comments",
			"rel": "comments"
		},
		{
			"likes-href": "/messages/1/likes",
			"rel": "likes"
		},
		{
			"shares-href": "/messages/1/shares",
			"rel": "shares"
		},
		{
			"profile-href": "/messages/1/adam",
			"rel": "author"
		}	
	]
}
```

# Maturity Model

Is this API "fully RESTful?"
Richardson Maturity Model

Level 3

Level 2

Level 1

#### Level 0: Create Message Request
One URL, Request body contains all the details 

Client ------> /messenger
Request Body

```XML
<create-message>
	<message-content>Hello World!</message-content>
	<message-author>Adam</message-author>
</create-message>
```
Swamp of POX 

#### Level 1:
Resource URI, Individual URIs for each resource

#### Level 2:
HTTP Methods
Uses the right HTTP methods, status codes

#### Level 3:
HATEOAS, reponses have links that the clients can use


















