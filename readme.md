# ES 8.4

## Index (like MySQL Table)

### Create Index
```
PUT localhost:9200/dqn_index
```

### Create Index Mapping
```
PUT localhost:9200/dqn_index/_mapping
{
    "properties": {
        "name": {
            "type": "text"
        }
    }
}
```

Field types - https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html

### Delete Index
```
DELETE localhost:9200/dqn_index
```

### Get All Indices
```
GET localhost:9200/_cat/indices?v
```

### Get All Indices with their mappings
```
GET http://localhost:9200/_mapping
```

## Document (Like a MySQL table row)

### Create Document
Create document for index 'dqn_index' with manually chose id 1. If you execute without last parameter, id will be generated automatically.
```
POST localhost:9200/dqn_index/_doc/1
{
    "name": "Dqn Dqnov"
}
```

### Get Document by ID
```
GET localhost:9200/dqn_index/_source/1
```

### Get All Documents by index
```
GET localhost:9200/dqn_index/_search?q=*:*
```

### Delete Document by ID
```
DELETE localhost:9200/dqn_index/_doc/1
```

## Roles and Privileges
First, you need to enable ``xpack.security.enabled: true`` from **/usr/share/elasticsearch/config/elasticsearch.yml**

Configure user and password:
```
ELASTIC_USERNAME=elastic
ELASTIC_PASSWORD=elastic
```

After that all above request must be sent with --user data or base64 (USER:PASSWORD) with header authorization.

### Create Privilege
```
POST localhost:9200/_security/privilege
--header 'Authorization: Basic ZWxhc3RpYzplbGFzdGlj'
{
  "app02": {
    "all": {
      "actions": [ "*" ]
    }
  }
}
```

This request can be used for update also. If you create a privilege, response looks like:

```
{
    "app02": {
        "all": {
            "created": true
        }
    }
}
```

If you run again, you will update it and response will be ```"created": false```.

### Create Role
Documentation - https://www.elastic.co/guide/en/elasticsearch/reference/current/security-api-put-role.html
```
PUT localhost:9200/_security/role/<role_name>
{
  "cluster": ["all"],
  "indices": [
    {
      "names": [ "*" ],
      "privileges": ["all"]
    }
  ]
}
```

### Get Role

```
GET localhost:9200/_security/role/<role_name>
```

### Create User
```
PUT localhost:9200/_security/user/<username>
{
  "password" : "123456",
  "roles" : [ "admin_role", "other_role1" ],
  "full_name" : "Deyan Deyanov",
  "email" : "deyan@pakov.com"
}
```

### Get User
```
GET localhost:9200/_security/user/<username>
```

Response:

```
{
    "dpakov": {
        "username": "dpakov",
        "roles": [
            "admin_role",
            "other_role1"
        ],
        "full_name": "Deyan Deyanov",
        "email": "deyan@pakov.com",
        "metadata": {},
        "enabled": true
    }
}
```

Or for all users:

```
GET localhost:9200/_security/user
```