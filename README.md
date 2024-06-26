# chat-app-chellenge (RFC)

## Context
We want to build a chat app , 3 main entities as a deafult , Application , chat and messages. The challenge include using Ruby on rails , redis , elastic search , go and a messageing queue more about the requirements in the follwoing section.

## Functional Requirments 
- Create Create , Read and Update endpoints for applications chats and messages Resources
- Required to provide API's only
- Each application will have a token (generated by the system) and it identifies the appliaction.
- Application will have token provided by the system and a name provided by the client
- Each application can have many chants.
- the application table should contain chat_count and it should be updated within 1 hour. may be not real time .
  
- each chat can have many messages
- chats have a number starting from 1 ..
- number of the chat should be returned in the creation response.
- no chats in the same application have the same number.
- clients ideintify chat by it's number along with it's application token.
- the chat table should contain messages_count and it should be updated within 1 hour. may be not real time .

- messages have numbers starting from 1 for each chat.
- the number of the message should be returned in the message creation.
- clients ideintify message by it's number along with it's chat number along with application token
- we can search in the messages partially using elastic search.

## Non-Functional Requirments
- All entities IDs should be hidden from the user (requests and responses)
- system will recivee many requests.
- system might be running on multiple servers in parallel and many requests can be processed concurrently
- minimize the quiries and avoid write dirctly to MYSQL especially for creation endpoints for chat and messages
- it's allowed for chats and messages to take time to be presisted.
- the application should be containerized. and using docker-compose-up to run the whole stack .
  
## High Level Design 
![High Level Design](https://github.com/xaaksw/chat-app-chellenge/blob/master/high-level-design.jpeg)
## DB Design 
![DB Design](https://github.com/xaaksw/chat-app-chellenge/blob/master/db-design.jpg)

## API Design
### Application Api's
#### Create Application: 
```
- EndPoint Url :  POST /api/applications
- Body : "name" : "XaakswIphone"
- Response status : 201
- ResponseBody: "token" : "xyzksja", "name" : "xaakswIphone"
```
#### Read Application(getChats): 
```
- EndPoint Url :  GET /api/applications/{token}
- Response status : 200
- ResponseBody: "token" : "xyzksja", "name" : "xaakswIphone","chatCount" : 10
```
#### Update Application(update name): 
```
- EndPoint Url :  PUT /api/applications/{token}
- Body: "name":"AhmedIphone",
- Response status : 204
```
### Chat Api's
#### Create Chat: 
```
- EndPoint Url :  POST /api/applications/{token}/chats
- Body : "name" : "XaakswChat" // idk if it's allowed to do this
- Response status : 201
- ResponseBody: "chatNumber":1
```
#### Read Chat(getMessages): 
```
- EndPoint Url :  GET /api/applications/{token}/chats/{chatNumber}
- Response status : 200
- ResponseBody: "chatNumber":1,"name":"XaakswChat","messagesCount" : 20
```
#### Update Chat(update name): 
```
- EndPoint Url :  PUT /api/applications/{token}/chats/{chatNumber}
- Body: "name":"AhmedChat",
- Response status : 204
```
### Messages Api's
#### Create Message: 
```
- EndPoint Url :  POST /api/applications/{token}/chats/{chatNumber}/messages
- Body : "body" : "Hello world!"
- Response status : 201
- ResponseBody: "messageNumber":1
```
#### Read Chat(getMessageBody): 
```
- EndPoint Url :  GET /api/applications/{token}/chats/{chatNumber}/messages/{messageNumber}
- Response status : 200
- ResponseBody: "messageNumber":1,"body":"hello world"
```
#### Update Message(update message body): 
```
- EndPoint Url :  PUT /api/applications/{token}/chats/{chatNumber}/messages/{messageNumber}
- Body: "body" : "Hey Man!",
- Response status : 204
``` 

