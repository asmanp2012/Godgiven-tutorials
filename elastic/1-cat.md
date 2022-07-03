# Elastic search learning - console tools

Elastic use with API and console and need both types of tools that users can use, in this section, we check console tools that you should not use in the application.

## [Cat command(Compact and aligned text APIs)](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat.html#cat)

Human eyes, especially when looking at a terminal, need compact and aligned text. The compact and aligned text (CAT) APIs aim to meet this need.

you can use cat with start command with `_cat` and put type of information that you need after that.

- `master`: `GET _cat/master` give you master node like `GET _nodes/_master`.
- `nodes`: `GET _cat/nodes` give you all node like `GET _nodes/_all`.
- `indices`: `GET _cat/indices` give you all indexs like `GET _stats`.

cat have some param after the command that you can use like bellow:

- `v`: the response includes column headings. Defaults to false. `GET _cat/master?v=true`
- `s`: Comma-separated list of column names or column aliases used to sort the response.
- `h`: Comma-separated list of column names to display. `GET _cat/master?h=host`
