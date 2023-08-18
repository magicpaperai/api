## Magic Paper API

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

#### Routes

<details>
 <summary><code>POST</code> <code><b>/api/v1/read</b></code> <code>(returns the extracted markdown for given inputs)</code></summary>

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
