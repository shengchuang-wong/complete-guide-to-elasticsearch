This repository contains all of the queries used within the [Complete Guide to Elasticsearch course](https://l.codingexplained.com/course/elasticsearch?src=github).

Github Repo
---
https://github.com/codingexplained/complete-guide-to-elasticsearch

Installation
---
1. Download Elasticsearch and Kibana
2. `tar -zxf <<tar.gz>>` to unzip

Run
---
- `bin/elasticsearch` - default port 9200
- `bin/kibana` - default port 5601

Node
---
- Cluster can contain multiple node
- replica shards must work in multiple nodes environment, since having only one node is possible if the node is down
- replice shard can be serve for quering since it contain the exact same data in the data shards
- one node can contain multiple replica shards, and each of the replice shards can be query at the same time, but limit but the core of the CPU. 4 core allow at most 4 replica shards to be query at the same time
- To add another node in cluster, since download a new elasticsearch and change the name and launch or you can run command in the same directory
```
bin/elasticsearch -Enode.name=name<<node_name>> Epath.data="./node-3/data -Epath.logs=./node-3/logs"
```

Doc
---
- document is immutable, it is being replaced instead 

Read
---
- read from best shard (there is an algorithnm for that)

Write
---
- write to primary shard
- primary shard will send a parallel request to replice shard to sync the newly data
- if primary shard is down, there is a recovery process
- primary terms = increment by how many shared is down
- sequence number = increment by how many action is perform
- global checkpoint for each replication group
- local checkpoint in each replica shard
- global checkpoints is the least value for all sequence number in all replication group's shards
- likewise, for local checkpoints, if any replica shards failed, only shard with higher sequence number need to applied it changes when the downed shard is recovered

FAQ
---
- Index vs Created - Created will failed if the data with id exist
- When using bulk action, Content-Type need to set as 'application/x-ndjson', and must end with \n or \r\n
- `curl -H "Content-Type: application/x-ndjson" -POST http://localhost:9200/products/_bulk --data-binary "@products-bulk.json"`
- if disabled dynamic mapping, then you will have to manually refresh the mapping when modify/add mapping, e.g. `POST /products/_udpdate_by_query?conflicts=proceed`

Characters Filter
---
1. HTML Strip Character Filter
2. Mapping Character Filter
3. Pattern Replace

Tokenizers
---
1. Word Oriented Tokenizers
- Standard Tokenizer
- Letter Tokenizer
- Lowercase Tokenizer
- Whitespace Tokenizer
- UAX URL Email Tokenizer
2. Partial Word Tokenizers
- N-Gram Tokenizer
- Edge N-Gram Tokenizer
3. Structured Text Tokenizers
- Keyword Tokenizer
- Pattern Tokenizer
- Path Tokenizer

Token Filters
---
1. Standard Token Filter
2. Lowercase Token Filter
3. NGram Token Filter
4. Stop Token Filter
5. Word Delimiter Token Filter
6. Stemmer Token Filter
7. Keyword Maker Token Filter
8. Snowball Token Filter
8. Synonym Token Filter

Analyzer
---
1. Standard Analyzer
2. Simple Analyzer
3. Stop Analyzer
4. Language Analyzers
5. Keyword Analyzer
6. Pattern Analyzer
7. Whitespace Analyzer

Relevance Score
---
1. Term Frequency (term appear within the document)
2. Inverse Document Frequency (term appear within index - all documents)
3. Field-length norm (how long is the field)

Searching
---
1. Term query - match exact word (not inverted index)
2. Match query - the search is analyze (inverted index)
3. wildcard search might be slow, especially when it is happen in the beginning of text
4. elasticsearch doesn't support full regex
5. Filter doesn't not affect relevance score, but it get cache when query, so good for performance

Join fields limitation
---
1. Documents must be stored within the same index
2. Parent & child documents must be on the same shard
3. Only one join field per index
- A join field can have as many relations as you want
- New relations can be added after creating the index
- Child relations can only be added to existing parents
4. Only one parent document
