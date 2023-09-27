# API

```
POST  /chat-messages
```

### Create chat message

Create a new conversation message or continue an existing dialogue.

#### Request Body

`inputs` _object_

(Optional) Provide user input fields as key-value pairs, corresponding to variables in Prompt Eng. Key is the variable name, Value is the parameter value. If the field type is Select, the submitted Value must be one of the preset choices.

`query` _string_&#x20;

User input/question content

`response_mode` _string_

- Blocking type, waiting for execution to complete and returning results. (Requests may be interrupted if the process is long)
- streaming returns. Implementation of streaming return based on SSE （[**Server-Sent Events**](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events)）.

`conversation_id` _string_

(Optional) Conversation ID: leave empty for first-time conversation; pass conversation_id from context to continue dialogue.

`user` _string_

The user identifier, defined by the developer, must ensure uniqueness within the app.

<pre class="language-markup" data-title="Request" data-overflow="wrap"><code class="lang-markup">
curl --location --request POST 'https://reiki-dev.web3go.xyz/ai/v1/chat-messages' \
--header 'Authorization: Bearer ENTER-YOUR-SECRET-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "inputs": {},
    "query": "eh",
    "response_mode": "streaming",
    "conversation_id": "1c7e55fb-1ba2-4e10-81b5-30addcea2276",
    "user": "abc-123"
}'
</code></pre>

blocking

<pre class="language-markup" data-title="Response" data-overflow="wrap">
<code class="lang-markup">
{  
    "answer": "Hi, is there anything I can help you?", 
    "conversation_id": "45701982-8118-4bc5-8e9b-64562b4555f2",  
    "created_at": 1679587005,
    "id": "059f87d9-15c0-473a-870c-fde95cdcc1e4"}

</code></pre>

streaming


```
  data: {"id": "5ad4cb98-f0c7-4085-b384-88c403be6290", "answer": " I", "created_at": 1679586595} 
  
  data: {"id": "5ad4cb98-f0c7-4085-b384-88c403be6290", "answer": " I", "created_at": 1679586595}
```

---
---
```
POST  /messages/{message_id}/feedbacks
```
### Message terminal user feedback, like

Rate received messages on behalf of end-users with likes or dislikes. This data is visible in the Logs & Annotations page and used for future model fine-tuning.

#### Path Params

`message_id` _string_

Message ID

#### Request Body

`rating` _string_

like or dislike, null is undo

`user` _string_

The user identifier, defined by the developer, must ensure uniqueness within the app.

<pre class="language-markup" data-title="Request" data-overflow="wrap"><code class="lang-markup">curl --location --request POST 'https://reiki-dev.web3go.xyz/ai/v1/messages/{message_id}/feedbacks \
 --header 'Authorization: Bearer ENTER-YOUR-SECRET-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "rating": "like",
    "user": "abc-123"
}'
</code></pre>
 

``` 
{  
  "has_more": false,  
  "data": [    
            {      
              "id": "WAz8eIbvDR60rouK",      "conversation_id": "xgQQXg3hrtjh7AvZ",      
              "inputs": {},      
              "query": "...",      
              "answer": "...",      
              "feedback": "like",      
              "created_at": 692233200    
            },    
            {  "id": "hSIhXBhNe8X1d8Et"      // ...    
            }  
          ]
}
```
 

---
---

```
GET  /messages
```


### Get the chat history message

The first page returns the latest `limit` bar, which is in reverse order.

#### Query

`conversation_id` _string_

Conversation ID

`first_id` _string_

ID of the first chat record on the current page. The default is none.

`limit` _int_

How many chats are returned in one request

`user` _string_

The user identifier, defined by the developer, must ensure uniqueness within the app.

<pre class="language-markup" data-title="Request" data-overflow="wrap"><code class="lang-markup">

<strong>
curl --location --request GET 'https://reiki-dev.web3go.xyz/ai/v1/messages?user=abc-123&#x26;conversation_id='\
</strong> --header 'Authorization: Bearer ENTER-YOUR-SECRET-KEY'
</code></pre>
 

```markup
{
	"has_more": false,
	"data": [{
		"id": "WAz8eIbvDR60rouK",
		"username": "FrankMcCallister",
		"phone_number": "1-800-759-3000",
		"avatar_url": "https://assets.protocol.chat/avatars/frank.jpg",
		"display_name": null,
		"conversation_id": "xgQQXg3hrtjh7AvZ",
		"created_at": 692233200
	}, {
		"id": "hSIhXBhNe8X1d8Et"
	}]
}
```
 

---
---
```
GET /conversations
```
 

### Get conversation list

Gets the session list of the current user. By default, the last 20 sessions are returned.

#### Query

`last_id` _string_

The ID of the last record on the current page, default none.

`limit` _int_

How many chats are returned in one request

`user` _string_

The user identifier, defined by the developer, must ensure uniqueness within the app.

{% code title="Request" overflow="wrap" %}

```markup

curl --location --request GET 'https://reiki-dev.web3go.xyz/ai/v1/conversations?user=abc-123&last_id=&limit=20'
```
 
 

```markup
{
	"limit": 20,
	"has_more": false,
	"data": [{
		"id": "10799fb8-64f7-4296-bbf7-b42bfbe0ae54",
		"name": "New chat",
		"inputs": {
			"book": "book",
			"myName": "Lucy"
		},
		"status": "normal",
		"created_at": 1679667915
	}, {
		"id": "hSIhXBhNe8X1d8Et"
	}]
}
```
 

---
---
```
POST  /conversations/{converation_id}/name
```
 

### Conversation renaming

Rename conversations; the name is displayed in multi-session client interfaces.

#### Request Body

`name` _string_

New name

`user` _string_

The user identifier, defined by the developer, must ensure uniqueness within the app.

<pre class="language-markup" data-title="Request" data-overflow="wrap"><code class="lang-markup">POST/conversations/{converation_id}/name

<strong>
curl --location --request POST 'https://reiki-dev.web3go.xyz/ai/v1/conversations/name' \
</strong>--header 'Authorization: Bearer ENTER-YOUR-SECRET-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{ 
 "name": "", 
 "user": "abc-123"
}'
</code></pre>


```markup
{  "result": "success"}
```


---
---
```
DELETE /conversations/{converation_id}
```


### Conversation deletion

Delete conversation.

#### Request Body

`user` _string_

The user identifier, defined by the developer, must ensure uniqueness within the app.


```markup

curl --location --request DELETE 'https://reiki-dev.web3go.xyz/ai/v1/conversations/{conversation_id}' \
--header 'Authorization: Bearer ENTER-YOUR-SECRET-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
 "user": "abc-123"
}'
```


```markup
{  "result": "success"}
```

