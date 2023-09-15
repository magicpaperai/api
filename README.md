## Magic Paper API

The Magic Paper API exposes scraping, embedding, and processing pipelines in a
few easy to use high-level endpoints.

------------------------------------------------------------------------------------------

#### Headers (provide all of these)

| name      |  expected value |
|-----------|-----------|
| `Authorization`   | `Bearer $SECRET_KEY` |
| `Content-Type`    |  `application/json`  | 

#### Base parameters (provide one of these)

| name      | description |
|-----------|-----------|
| `text`    |  plain-text or markdown to process  |
| `html`    |  html to parse and process  |
| `url`     |  url to scrape, parse and process  |

#### Format parameter (optional)

| name      | description |
|-----------|-----------|
| `format`    |  `markdown` to parse html in markdown, default is just plaintext  |

#### Routes

<details>
 <summary><code>POST</code> <code><b>/api/v1/read</b></code> <code>(returns the parsed text for given inputs)</code></summary>

##### Parameters

> No additional parameters

##### Example Response

> ```json
> {
>     "title": "Markdown Article",
>     "text": "# Hello World\n"
> }
> ```

##### Example CURL

> ```sh
> curl -X POST \
>   -d '{"url": "http://blog.mattneary.com/worse-is-better"}' \
>   -H 'Content-Type: application/json' \
>   -H 'Authorization: Bearer sk-0000-0000-0000' \
>   http://alpha.magicpaper.ai/api/v1/read
> ```

</details>

<details>
 <summary><code>POST</code> <code><b>/api/v1/salience</b></code> <code>(extracts most salient fragments)</code></summary>

##### Parameters

> | name      |  type     | data type               | description                                                           |
> |-----------|-----------|-------------------------|----------------------------|
> | `limit`     |  optional | integer   | the max number of fragments to return |


##### Example Response

> ```json
> {
>     "sentences": [
>         {
>             "sentence": "The quick brown fox jumped over the lazy dog.",
>             "interval": [0, 45],
>             "score": 1.0
>         }
>     ]
> }
> ```

##### Example CURL

> ```sh
> curl -X POST \
>   -d '{"url": "http://blog.mattneary.com/worse-is-better"}' \
>   -H 'Content-Type: application/json' \
>   -H 'Authorization: Bearer sk-0000-0000-0000' \
>   http://alpha.magicpaper.ai/api/v1/salience
> ```

</details>

<details>
 <summary><code>POST</code> <code><b>/api/v1/search</b></code> <code>(extracts fragments most relevant to query)</code></summary>

##### Parameters

> | name      |  type     | data type               | description                                                           |
> |-----------|-----------|-------------------------|----------------------------|
> | `query`     |  required | string or array of strings   | strings to compare text against |
> | `limit`     |  optional | integer   | the max number of fragments to return |
> | `method`     |  optional | `avg` or `min`   | methodology to use in comparing against array of queries |


##### Example Response

> ```json
> {
>     "sentences": [
>         {
>             "sentence": "The quick brown fox jumped over the lazy dog.",
>             "interval": [0, 45],
>             "score": 1.0
>         }
>     ]
> }
> ```

##### Example CURL

> ```sh
> curl -X POST \
>   -d "{\"text\": \"$ARTICLE_TEXT\", \"query\": \"history of technology\"}" \
>   -H 'Content-Type: application/json' \
>   -H 'Authorization: Bearer sk-0000-0000-0000' \
>   http://alpha.magicpaper.ai/api/v1/search
> ```

</details>

<details>
 <summary><code>POST</code> <code><b>/api/v1/cluster</b></code> <code>(extracts fragments in topic clusters)</code></summary>

##### Parameters

> | name      |  type     | data type               | description                                                           |
> |-----------|-----------|-------------------------|----------------------------|
> | `limit`     |  optional | integer   | the max number of clusters to return (default 6) |


##### Example Response

> ```json
> {
>     "clusters": [
>         {
>             "sentences": ["The quick brown fox jumped over the lazy dog."],
>             "intervals": [[0, 45]],
>             "title": ["quick brown fox, lazy dog"],
>             "hue": 0.0
>         }
>     ]
> }
> ```

##### Example CURL

> ```sh
> curl -X POST \
>   -d "{\"url\": \"$url\", \"limit\": 2}" \
>   -H 'Content-Type: application/json' \
>   -H 'Authorization: Bearer sk-0000-0000-0000' \
>   http://alpha.magicpaper.ai/api/v1/cluster
> ```

</details>

<details>
 <summary>🧪 <code>POST</code> <code><b>/api/v1/experiments/salience</b></code> <code>(updated salience algorithm)</code></summary>

##### Parameters

> | name      |  type     | data type               | description                                                           |
> |-----------|-----------|-------------------------|----------------------------|
> | `limit`     |  optional | integer   | the max number of fragments to return (default 5) |
> | `temperature`     |  optional | float   | higher temperature has more breadth, lower more depth |


##### Example Response

> ```json
> {
>   "prompt": "The following are key excerpts from a text. Some context will be missing, so do not assume that these excerpts are a complete representation of the orgiinal text. If the text is well-known, use the chosen excerpts to produce a summary. Otherwise, based on the excerpts and title, do your best to summarize the original text. Your response should be written like a summary of the text, with no mention of excerpts.\n\n\n## Title\nWorse Is Better\n\n## Author(s)\nNone\n\n## Excerpts\n- And it’s possible to design systems with the big picture in mind.\n- The most important problem in system design is making sure it will evolve effectively, and it’s as much a social problem as a technical one.\n\n## Summary",
>   "sentences": [
>     {
>       "block": 10,
>       "range": [
>         4284,
>         4349
>       ],
>       "sentence": "And it’s possible to design systems with the big picture in mind."
>     },
>     {
>       "block": 12,
>       "range": [
>         4993,
>         5133
>       ],
>       "sentence": "The most important problem in system design is making sure it will evolve effectively, and it’s as much a social problem as a technical one."
>     }
>   ]
> }
> ```

##### Example CURL

> ```sh
> curl -X POST \
>   -d '{"url": "http://blog.mattneary.com/worse-is-better", "limit": 2, "temperature": 0}' \
>   -H 'Content-Type: application/json' \
>   -H 'Authorization: Bearer sk-0000-0000-0000' \
>   http://alpha.magicpaper.ai/api/v1/experiments/salience
> ```

</details>
