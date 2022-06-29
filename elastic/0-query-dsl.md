# Elastic search learning - Query DSL

Elastic search work by `Query DSL`(java database query) that elastic sync that with api endpoint and you should work with it on kibana and Restfull api.

## Install kibana

In the first install kibana on the server ( it serve for you a web aplication that you can work with it ) and go to:
`Mnagement -> Dev tools`

When you open that show a console that can run bellow commands and show the result of it.

## Insert

```elastic
PUT sample_index/_doc/1
{ 
    "title" : "Sample title", 
    "date" : "2019-08-15T14:12:12", 
    "description" : "Sample des" 
}
```

- `sample_index`: is your index
- `_doc`: work as a key for endpoint
- `1`: is your data

## Read by index

```elastic
GET sample_index/_doc/1
```

- `sample_index`: is your index
- `_doc`: work as a key for endpoint
- `1`: is your data
