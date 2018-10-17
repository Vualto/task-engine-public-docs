# Reseller API

The reseller-api allows resellers to manage their own clients programmatically.  This is a restful API.

## Basic requirements

The reseller must already have a client account within the platform in order to have a valid API key AND there must also be AT LEAST one reseller user present for that client.

All requests require the following headers to be set:

* `Reseller`: reseller client name
* `Api-Key`: reseller client api key  
* `Content-Type`: `application/json`

## Clients

The `/client` endpoint allows manipulation of the client with one call.  When creating a client, all of the necessary parameters can be passed, including resource URL's and user details.  When setting values remember that everything is stored as a STRING **DO NOT** try to set numberic values (i.e. send "x": "1" rather than "x": 1).

Clients have three main properties: categories, resources and users.

#### Categories

Categories are a named group of key/value pairs.  The main point to be aware of when manipulating clients via `POST /client` or `PUT /client` is that the supplied category key/values **REPLACES** any existing values.  For example, if you created a client (`POST /client`) with these values:

```
{
# ...
	"categories": {
		"demo": {
			"a": "aaa",
			"b": "bbb"
		}
	}
# ...
}
```

and subsequently updated the client (`PUT /client`) with these values:

```
{
# ...
	"categories": {
		"demo": {
			"c": "ccc"
		}
	}
# ...	
}
```

then the `demo` category would only be left with the values `"c": "ccc"`, the previously set `aaa` and `bbb` keys would be removed. 

There is more fine-grained control of categories available at the [categories endpoint](#categories_1)

#### Resources

Resources should be speciified with a public reachable url.

#### Users
Users should have an email address and a password set.  Once set passwords are **NOT** retrievable, the user will either have to use the password reminder feature in admin OR the [users endpoint](#users_1) can be used to update the users details.
 
## GET /client/{client_name}

**Example response:**

Headers:

```
Content-Type: application/json
Vualto-Transaction-Id: 716ce40b-1111-2222-3333-f7cd1ad7aeff
```

Body:

```
{
  "client": {
    "name": "example",
    "providers": "playready,widevine,access",
    "categories": {
      "default": {
        "sharedsecret": "examplesecret",
        "timezone": "Europe/London",
        "tokenkeepalive": "3600",
        "cache_expiry_seconds": "30",
        "api_key": "examplekey"
      },
      "playready": {
        "keyseed": "somevalue",
        "userkey": "someguid",
        "laurl": "https://playready-license.uat.drm.technology/rightsmanager.asmx",
        "securitylevel": "2000"
      },
      "widevine": {
        "test": "value1"
      },
      "demo": {
        "d1": "d1 value",
        "d2": "d2 value",
        "d3": "d3 value"
      }
    },
    "resources": {
      "bar": "https://example-s3.com/file1.txt",
      "foo": "https://exmaple-s3.com/file2.bin"
    },
    "users": [
      {
        "email": "xxx@example.com"
      },
      {
        "email": "yyy@example.com"
      }
    ]
  }
}
```

## POST /client

**Example request:**

Body: 

```
{
   "client":{
       "name": "example",
       "providers": "widevine,playready",
       "categories": {
           "widevine": {
               "test": "value1"    
           },
           "demo": {
               "d1": "d1 value",
               "d2": "d2 value"
           }
       },
       "resources": {
           "foo": "http://exmaple.com/file1.txt",
           "bar": "http://example.com/file2.bin"
       },
       "users": [
           {
               "email": "xxx@example.com",
               "password": "xxx"
           },
           {
               "email": "yyy@example.com",
               "password": "yyy"
           }        
       ]
   }
}
```

**Example response:**

Headers:

```
Content-Type: application/json
Vualto-Transaction-Id: 716ce40b-1111-2222-3333-f7cd1ad7aeff
```

Body:

```
{
  "result": "Client created"
}
```

## PUT /client

**NOTE:** Users cannot be manipulated via this endpoint - any `users` defined in the request body are ignored.  Use the [users endpoint](index.md#users_1) to achieve this.

**Example request:**

Body:

```
{
   "client":{
       "name": "example",
       "providers": "widevine,playready,access",
       "categories": {
           "demo": {
               "d1": "d1 value",
               "d2": "d2 value",
               "d3": "d3 value"
           }
       }
   }
}
```

**Example response:**

Headers:

```
Content-Type: application/json
Vualto-Transaction-Id: 716ce40b-1111-2222-3333-f7cd1ad7aeff
```

Body:

```
{
  "result": "Client updated"
}
```

## DELETE /client/{client name}

**NOTE:** Associated `users` and `resources` will be deleted.

**Example response:**

Headers:

```
Content-Type: application/json
Vualto-Transaction-Id: 716ce40b-1111-2222-3333-f7cd1ad7aeff
```

Body:

```
{
  "result": "Client deleted"
}
```

## Categories

Categories are a named group of key/value pairs.  The `/categories` endpoint allows fine-grained control of categories and their values - whereas `/client` replaced values `/categories` allows update of individual keys/values without affecting others in the same category.

## PUT /categories

This verb allows the insertion/manipulation and deletion of categories and their key/value pairs.

**Example request:**

In the below example the client doesn't currently have a `test` category, so this will be created and the key `t1` will be assigned the value `test1` in the newly created category.  Assuming that the `widevine` category DOES exist, then the request is setting (or inserting if it doesn't exist) the key `key` to the value `aaabbb111222` AND it is removing the key `t2` if it exists (no error is returned if it doesn't).

**NOTE:** When the last key for a category is deleted then the category is also deleted.

Body:

```
{
	"client": {
		"name": "example",
		"categories": {
			"test": {
				"t1": "test1"
			}, 
			"widevine": {
				"key": "aaabbb111222",	
				"t2": ""
			}
		}
	}
}
```
**Example response:**

Headers:

```
Content-Type: application/json
Vualto-Transaction-Id: 716ce40b-1111-2222-3333-f7cd1ad7aeff
```

Body:

```
{
  "result": "Updated"
}
```

## DELETE /categories

This will delete all requested categories.  If any of the categories don't exist no error will be returned.

**Example request:**

Body:

```
{
    "client": {
        "name": "example",
        "categories": ["test", "demo"]
    }
}
```

**Example response:**

Headers:

```
Content-Type: application/json
Vualto-Transaction-Id: 716ce40b-1111-2222-3333-f7cd1ad7aeff
```

Body:

```
{
  "result": "Deleted"
}
```

## Resources

## PUT /resources

If the defined key (`foo` in the example below) exists then the resource will be updated with the contents from the provided URL, otherwise a new resource `foo` is created.

**NOTE:** The API will retrieve the content from the provided URL and store a copy, so it is **VITAL** that the URL is publicly reachable **AND** is the content you want - the ONLY failure that will result in the resource not being upserted is a TCP connection error.

**Example request:**

Body:

```
{
    "client": {
        "name": "example",
        "resources":{
            "foo": "http://example.com/file1.txt"
        }
    }
}
```

**Example response:**

Headers:

```
Content-Type: application/json
Vualto-Transaction-Id: 716ce40b-1111-2222-3333-f7cd1ad7aeff
```

Body:

```
{
  "result": "Updated"
}
```

## DELETE /resources

Removes the requested resource(s) from the client.

**Example request:**

Body:

```
{
    "client": {
        "name": "example",
        "resources": ["foo", "bar"]
    }
}
```

**Example response:**

Headers:

```
Content-Type: application/json
Vualto-Transaction-Id: 716ce40b-1111-2222-3333-f7cd1ad7aeff
```

Body:

```
{
  "result": "Deleted"
}
```

## Users

## GET /users/{client name}

**Example response:**

Headers:

```
Content-Type: application/json
Vualto-Transaction-Id: 716ce40b-1111-2222-3333-f7cd1ad7aeff
```

Body:

```
{
  "users": [
    "xxx@example.com",
    "yyy@example.com"
  ]
}
```

## POST /user

**Example request:**

```
{
    "client": "example",
    "email": "zzz@example.com",
    "password": "zzz"
}
```

**Example response:**

Headers:

```
Content-Type: application/json
Vualto-Transaction-Id: 716ce40b-1111-2222-3333-f7cd1ad7aeff
```

Body:

```
{
  "result": "User created"
}
```

**OR**

If the user already exists a `400 Bad Request` is returned with the following:

```
{
  "error": "User already exists"
}
```

## PUT /user

This is the only way to update a user password via the API.  A users email address cannot be changed.

**Example request:**

Body:

```
{
    "client": "example",
    "email": "zzz@example.com",
    "password": "1234"
}
```

**Example response:**

Headers:

```
Content-Type: application/json
Vualto-Transaction-Id: 716ce40b-1111-2222-3333-f7cd1ad7aeff
```

Body:

```
{
  "result": "User updated"
}
```

**OR**

If the user doesn't exist a `400 Bad Request` is returned with the following:

```
{
  "error": "User not found"
}
```

## DELETE /user

**Example request:**

Body:

```
{
    "client": "example",
    "email": "zzz@example.com"
}
```

**Example response:**

Headers:

```
Content-Type: application/json
Vualto-Transaction-Id: 716ce40b-1111-2222-3333-f7cd1ad7aeff
```

Body:

```
{
  "result": "User deleted"
}
```

**OR**

If the user doesn't exist a `400 Bad Request` is returned with the following:

```
{
  "error": "User not found"
}
```
