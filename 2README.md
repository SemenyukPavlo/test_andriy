# FILES
## PATH TO FILES
### PATH TO FILES

#### PATH TO FILES
##### PATH TO FILES
###### PATH TO FILES
####### PATH TO FILES


```
/TYPE_OF_ENTITY/ID_OF_ENTITY/TYPE_OF_FILE/FILE_NAME

TYPE_OF_ENTITY: ['users', 'topics', 'comments', 'categories']

TYPE_OF_FILE: ['small', 'medium', 'large', 'original']
small - max width or height 400px
medium - max width or height 800px
large - max width or height 120px

```

## CATEGORY
```
request body
{
    pic: String // name of file property, if name is 'image' - in files should be object with property image
}
```

## USER
```
request body
{
    pic: String // name of file property
}
```

## TOPIC
```
request body
{
    ...
    data: [{
        type: 'img',
        data: String // name of file property
    }, {
        type: 'poll',
        data: {
            ...
            options: [
                ...
                pic: String // name of file property
            ]
        }
    }]
}
```

## COMMENTS
```
request body
{
    ...
    images: Array // list of files properties
}
```

# SOCKETS
## EMITS FROM SERVER
```
success_connection - send immediately after connect

new_comment - send after new comment in topic
data: {
    _id: String, // comment id
    text?: String,
    imgs?: Array, // array of path to images
    created_at: Date,
    reply?: String // reply comment,
    user_id: String, // id of comment owner,
    topic_id: String,
    random_client_id: String,
    user: Object
}

update_comment - send after comment was edited
data: {
    _id: String, // comment id
    text?: String,
    imgs?: Array, // array of path to images
    topic_id: String,
    random_client_id: String
}

remove_comment - send after comment was removed
data: {
    _id: String, // comment id
    topic_id: String,
    random_client_id: String
}

comment_like - send after comment like/unlike
data: {
    _id: String, // comment id
    topic_id: String,
    count: Number // count of likes,
    random_client_id: String
}
```

## EMITS FROM CLIENT
```
open_topic - send on topic open
data: {
    topic_id: String,
    random_client_id: String
}

Response body:
{
    comment_id: String,
    notifications_qty: Number,
    random_client_id: String
}

close_topic - send on topic close
data: {
    topic_id: String,
    comment_id: String,
    notifications_qty: Number,
    random_client_id: String
}

new_comment - send after new comment in topic
data: {
    text?: String,
    comment_id?: String // id of parent comment,
    topic_id: String,
    random_client_id: String
}

update_comment - send after comment was edited
data: {
    _id: String, // comment id
    text?: String,
    topic_id: String,
    random_client_id: String
}

remove_comment - send after comment was removed
data: {
    _id: String, // comment id
    topic_id: String,
    random_client_id: String
}

comment_like - send after comment like/unlike
data: {
    _id: String, // comment id
    topic_id: String,
    type: String, // like or dislike
    count: Number // count of likes,
    random_client_id: String
}

get_comments - list of comments
data: {
    topic_id: String,
    comment_id?: String,
    direction?: String, //next or prev
    random_client_id: String
}

Response body:
{
    list: Array, //list of comments
    prev: Boolean,
    next: Boolean,
    random_client_id: String
}

COMMENT MODEL:
_id: String, // comment id
text?: String,
imgs?: Array, // array of path to images
created_at: Date,
comment?: Object // data of parent comment,
user: Object,
is_edited: Boolean, //
is_liked: Boolean, // if user set like for comment
likes_qty: Number //count of likes
```

## COMMENT CREATE WITH IMAGES
### POST: /api/comments
```

data: {
    text?: String,
    images: [String], name of files properties, if name is 'image' - in files should be object with property image
    comment_id?: String // id of parent comment,
    topic_id: String,
    random_client_id: String
}

```

# Base URL
- http://5.187.0.233

# AUTH
## Retrieve user data by social network type
### GET: /api/{type}/token
```
FB
ClientID: '1908104719203949'
ClientSecret: '77d706f490883fba8d434f7f0cb90d97'

Google
ClientID: '987697904748-3ibgkg2v8dmcvkq7guvjsp0s566pcpd0.apps.googleusercontent.com'
ClientSecret: '6A8s0dzD0cMSIp9Nj4u01B2c'

Response Headers: {
	token: JWT_HERE
}

Response:
{
	success: true,
	data: {
		name: String,
		email: String,
		gender: String,
		location: String,
		birthday: String,
		pic: String
	}
}
```

# AUTHORIZED REQUESTS
```
JWT should be added to all requests that require authentication

Example:
URL: /api/users/categories
METHOD: GET
HEADERS: Authorization: Bearer JWT_HERE
```


# USERS
## Get user by id
### GET: /api/users/{user_id}
```
Response:
{
	success: true,
	data: {
		name: String,
		email: String,
		gender: String,
		location: String,
		birthday: String,
		pic: String,
		facebook_id: String,
		categories: Array, // array of user categories ids
		topics: Array, // array of user topics ids
		notInterested: Array // array of user not intereseted topics ids
	}
}
```

## Update user
### PUT: /api/users/{user_id}
```
Request body:
{
	name?: String,
	email?: String,
	gender?: String,
	location?: String,
	birthday?: String,
	pic?: String,
	facebook_id?: String,
	categories?: Array, // array of user categories ids
	topics?: Array, // array of user topics ids
	notInterested?: Array // array of user not intereseted topics ids
}

Response:
{
	success: true,
	data: Object // user data
}
```

## Add user device id
### POST: /api/users/appleDeviceId
```
Request body:
{
	apple_device_id: String
}

Response:
{
	success: true
}
```

# GATEGORIES
## Get list of categories
### GET: /api/categories
```
Query params:
include: type of categories (chosen, like, pop) //include=chosen, include=like,pop, include=chosen,like,pop
sub: if 'true' add subcategories to each category
main: if 'true' only main categories
search: text for search in category title

Response:
{
	success: true,
	data: {
		other: [{
			_id: String,
			title: String,
			pic: String,
			category_id: String, // parent category id
			sub?: [{
				_id: String,
				title: String,
				pic: String,
				category_id: String,
				sub?: [{}]
			}]
		}, {
			...
		}],
		chosen?: [{}], // array or user chosen categories
		like?: [{}], // array of interested categories for user
		pop?: [{}] // array of popular categories
	}
}
```

## Get category by id
### GET: /api/categories/{category_id}
```
Query params:
sub: if 'true' add subcategories to category

Response:
{
	success: true,
	data: {
		_id: String,
		title: String,
		pic: String,
		category_id: String, // parent category id
		sub: [{
			_id: String,
			title: String,
			pic: String,
			category_id: String,
			sub: [{}]
		}
	}
}
```

## Create category
### POST: /api/categories
```
Request body:
{
	title: String,
	pic: String,
	category_id?: String //parent category id
}

Response:
{
	success: true,
	data: Object // category data
}
```

## Update category
### PUT: /api/categories/{category_id}
```
Request body:
{
	title?: String,
	pic?: String,
	category_id?: String //parent category id
}

Response:
{
	success: true,
	data: Object // category data
}
```

# TOPICS
## List of topics for explore page
### GET: /api/topics/explore
```
Query params:
page: Number (20 topics for one page)

Response:
{
	success: true,
	data: [{
		title: String,
		pic?: String,
		data: [{
            type: 'text',
            data: 'some text'
        }, {
            type: 'img',
            data: 'path to image'
        }, {
            type: 'poll',
            data: {
                type: 'vs', // or single
                text: 'some question',
                options: [{
                    text?: '',
                    pic?: ''
                }, {
                    text?: '',
                    pic?: ''
                }]
            }
        }],
	  	likes_qty: Number,
		isLiked: Boolean, // if user set like,
		isPoll: Boolean, // if poll exist in topic
	  	created_at: Date,
		updated_at: Date,
  		user_id: String, // id of topic owner
		categories: Array of objects,
		users: Array, // array of direct users ids
		active_users: Array // array of active users ids (current topic is open),
	}, {
		...
	}]
}
```

## List of topics in history
### GET: /api/topics/history
```
Query params:
search: text for search in category title
page: Number (20 topics for one page)

Response:
{
	success: true,
	data: [{
		title: String,
		pic?: String,
		data: [{
            type: 'text',
            data: 'some text'
        }, {
            type: 'img',
            data: 'path to image'
        }, {
            type: 'poll',
            data: {
                type: 'vs', // or single
                text: 'some question',
                options: [{
                    text?: '',
                    pic?: ''
                }, {
                    text?: '',
                    pic?: ''
                }]
            }
        }],
	  	likes_qty: Number,
		isLiked: Boolean, // if user set like
		isPoll: Boolean, // if poll exist in topic
	  	created_at: Date,
		updated_at: Date,
  		user_id: String, // id of topic owner
		categories: Array of objects,
		users: Array, // array of direct users ids
		active_users: Array // array of active users ids (current topic is open)
	}, {
		...
	}]
}
```

## List of topics for direct page
### GET: /api/topics/direct
```
Query params:
search: text for search in category title
page: Number (20 topics for one page)

Response:
{
	success: true,
	data: [{
		title: String,
		pic?: String,
		data: [{
            type: 'text',
            data: 'some text'
        }, {
            type: 'img',
            data: 'path to image'
        }, {
            type: 'poll',
            data: {
                type: 'vs', // or single
                text: 'some question',
                options: [{
                    text?: '',
                    pic?: ''
                }, {
                    text?: '',
                    pic?: ''
                }]
            }
        }],
	  	likes_qty: Number,
		isLiked: Boolean, // if user set like
		isPoll: Boolean, // if poll exist in topic
	  	created_at: Date,
		updated_at: Date,
  		user_id: String, // id of topic owner
		categories: Array of objects,
		users: Array, // array of direct users ids
		active_users: Array // array of active users ids (current topic is open)
	}, {
		...
	}]
}
```

## List of favorite topics
### GET: /api/topics/favorite
```
Query params:
search: text for search in category title
page: Number (20 topics for one page)

Response:
{
	success: true,
	data: [{
		title: String,
		pic?: String,
		data: [{
            type: 'text',
            data: 'some text'
        }, {
            type: 'img',
            data: 'path to image'
        }, {
            type: 'poll',
            data: {
                type: 'vs', // or single
                text: 'some question',
                options: [{
                    text?: '',
                    pic?: ''
                }, {
                    text?: '',
                    pic?: ''
                }]
            }
        }],
	  	likes_qty: Number,
		isLiked: Boolean, // if user set like
		isPoll: Boolean, // if poll exist in topic
	  	created_at: Date,
		updated_at: Date,
  		user_id: String, // id of topic owner
		categories: Array of objects,
		users: Array, // array of direct users ids
		active_users: Array // array of active users ids (current topic is open)
	}, {
		...
	}]
}
```

## Get topic by id
### GET: /api/topics/{topic_id}
```
Response:
{
	success: true,
	data: {
		title: String,
		pic?: String,
		data: [{
            type: 'text',
            data: 'some text'
        }, {
            type: 'img',
            data: 'path to image'
        }, {
            type: 'poll',
            data: {
                type: 'vs', // or single
                text: 'some question',
                options: [{
                    text?: '',
                    pic?: ''
                }, {
                    text?: '',
                    pic?: ''
                }]
            }
        }],
	  	likes_qty: Number,
		isLiked: Boolean, // if user set like
		isPoll: Boolean, // if poll exist in topic
	  	created_at: Date,
		updated_at: Date,
  		user_id: String, // id of topic owner
		categories: Array of objects,
		users: Array, // array of direct users ids
		active_users: Array // array of active users ids (current topic is open)
	}
}
```

## Create topic
### POST: /api/topics
```
Request body:
{
	title: String,
	data?: [{
        type: 'text',
        data: 'some text'
    }, {
        type: 'img',
        data: 'path to image'
    }, {
        type: 'poll',
        data: {
            type: 'vs', // or single
            text: 'some question',
            options: [{
                text?: '',
                pic?: ''
            }, {
                text?: '',
                pic?: ''
            }]
        }
    }],
	pic?: String,
	category_id: String
}

Response:
{
	success: true,
	data: Object // topic data
}
```

## Update topic
### PUT: /api/topics/{topic_id}
```
Request body:
{
	title?: String,
	data?: [{
        type: 'text',
        data: 'some text'
    }, {
        type: 'img',
        data: 'path to image'
    }, {
        type: 'poll',
        data: {
            type: 'vs', // or single
            text: 'some question',
            options: [{
                text?: '',
                pic?: ''
            }, {
                text?: '',
                pic?: ''
            }]
        }
    }],
	pic?: String,
	category_id?: String
}

Response:
{
	success: true,
	data: Object // topic data
}
```

## Remove topic
### DELETE: /api/topics/{topic_id}
```
Response:
{
	success: true
}
```

## Add topic to favorite
### POST: /api/topics/favorite
```
Request body:
{
	type: String, // 'add' if add and empty in no
	topic_id: String
}

Response:
{
	success: true
}
```

## Add topic to not interested
### POST: /api/topics/notInterested
```
Request body:
{
	topic_id: String
}

Response:
{
	success: true
}
```

## Set like
### POST: /api/comments/like
```
Request body:
{
	type: String, // 'like' if like and empty in no
	comment_id: String
}

Response:
{
	success: true
}
```

# POLL
## Vote
### POST: /api/polls/vote
```
Request body:
{
	poll_id: String,
	option_id: String
}

Response:
{
	success: true
}
```
