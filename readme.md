# ES 8.4

## Index (like MySQL Table)

### Create Index
```
PUT localhost:9200/dqn_index
```

### Create Index Mapping
```
PUT localhost:9200/dqn_index/_mapping
-d '{
    "properties": {
        "name": {
            "type": "text"
        }
    }
}'
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
curl POST localhost:9200/dqn_index/_doc/1
-d '{
    "name": "Dqn Dqnov"
}'
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