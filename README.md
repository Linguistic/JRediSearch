# JRediSearch

Redis with full-text search support, thanks to Java and [RediSearch](https://redisearch.io).

## This is a Fork
Yes. It's true. We were not dedicated enough to try to write our own implementation of this library, so we forked the
original. Everything outside of this blurb is from the original repository, pretty much word-for-word. 
Now, why did we fork it, that is, apart from our sheer laziness? Well, we weren't the biggest fans of how this repository
was maintained. All of the JAR dependencies were kept local, so you couldn't even pull the latest version from Maven. 
What's up with that? Also, there was no way to get this library apart from building it yourself and adding it to you app.
This way, we can at least publish releases to the GitHub Releases tab. So, we went ahead and replace the POM-based dependency
management this app used with [Facebook Buck](https://buckbuild.com). Now, we can more effectively manage this library's
dependencies. The only JAR remaining in this app is the Jedis v3.0 JAR, seeing there was no way to download it from the 
Jedis repo, and the Maven version is grossly outdated. So this may be the only copy in existence. Who knows?

As for testing, don't ask. You can run Buck tests, but it relies on a local server running on two different ports and 
there were no instructions in the original repo so just... don't. Don't ask. Maybe you can help us rewrite it. 

Anyway, to build:

```bash
$ ./build.sh
```

or:

```bash
$ buck build it --out <custom_output_path>
```

and to test (not that you'd want to):

```bash
$ buck test it
```

## Overview 

This project contains a Java library abstracting the API of the RediSearch Redis module, that implements a powerful 
in-memory search engine inside Redis. 
 
See [http://redisearch.io](http://redisearch.io) for installation instructions of the module.

## Usage example

Initializing the client:

```java

import io.redisearch.client.Client;
import io.redisearch.Document;
import io.redisearch.SearchResult;
import io.redisearch.Query;
import io.redisearch.Schema;

...

Client client = new Client("testung", "localhost", 6379);

```

Defining a schema for an index and creating it:

```java

Schema sc = new Schema()
                .addTextField("title", 5.0)
                .addTextField("body", 1.0)
                .addNumericField("price");

client.createIndex(sc, Client.IndexOptions.Default());

```
 
Adding documents to the index:

```java
Map<String, Object> fields = new HashMap<>();
fields.put("title", "hello world");
fields.put("body", "lorem ipsum");
fields.put("price", 1337);

client.addDocument("doc1", fields);

```

Searching the index:

```java

// Creating a complex query
Query q = new Query("hello world")
                    .addFilter(new Query.NumericFilter("price", 0, 1000))
                    .limit(0,5);

// actual search
SearchResult res = client.search(q);


```

---
 
### Also supported:

* Geo filtering
* Exact matching
* Union matching
* Stemming in 17 languages
* Deleting and updating documents on the fly
* And much more... See [http://redisearch.io](http://redisearch.io) for more details.

