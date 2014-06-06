# 更新整个文档

Documents in Elasticsearch are immutable -- we cannot change them. Instead, if
we need to update an existing document, we _reindex_ or replace it, which we
can do using the same `index` API that we have already discussed in
<<index-doc>>.

```js
PUT /website/blog/123
{
  "title": "My first blog entry",
  "text":  "I am starting to get the hang of this...",
  "date":  "2014/01/02"
}
```

In the response, we can see that Elasticsearch has incremented the `_version`
number:

```js
{
  "_index" :   "website",
  "_type" :    "blog",
  "_id" :      "123",
  "_version" : 2,
  "created":   false <1>
}
```
1. The `created` flag is set to `false` because a document with the same index, type and ID already existed.

Internally, Elasticsearch has marked the old document as deleted and added an
entirely new document. The old version of the document doesn't disappear
immediately, although you won't be able to access it. Elasticsearch cleans up
deleted documents in the background as you continue to index more data.

Later in this chapter, we will discuss the `update` API, which can be used to
make <<partial-updates,partial updates to a document>>. This API *appears* to
change documents in place, but actually Elasticsearch is following exactly the
same process as described above:

1. retrieve the JSON from the old document
2. change it
3. delete the old document
4. index a new document

The only difference is that the `update` API achieves this through a single
client request, instead of requiring separate `get` and `index` requests.

