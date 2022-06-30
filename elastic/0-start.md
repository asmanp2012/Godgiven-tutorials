# Elastic search learning - Query DSL

Elastic search work by `Query DSL`(java database query) that elastic sync that with api endpoint and you should work with it on kibana and Restfull api.

## Install kibana

In the first install kibana on the server ( it serve for you a web aplication that you can work with it ) and go to:
`Management -> Dev tools`

When you open that show a console that can run bellow commands and show the result of it.

`sample_index`: is name key that we chose for `index`

## [Check the health of elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-health.html)

you can check the health of elastic with `health` endpoint.

```elastic
GET _cluster/health`
```

## [Insert](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html)

```elastic
PUT sample_index/_doc/1
{ 
    "title" : "Sample title", 
    "date" : "2019-08-15T14:12:12", 
    "description" : "Sample des" 
}
```

- `_doc`: reference to data
- `1`: index of your data

## [Read by index](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-get.html)

```elastic
GET sample_index/_doc/1
```

- `_doc`: reference to data
- `1`: index of your data

## [give the statics of index](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-stats.html)

```elastic
Get sample_index/_stats
```
