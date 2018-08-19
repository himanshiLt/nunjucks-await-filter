<h3 align="center">Nunjucks <code>await</code> Filter</h3>

## Usage

Add the filter to your Nunjucks environment, and prepare an `async` function:

```js
const awaitFilter = require('nunjucks-await-filter')
const environment = nunjucks.configure()
awaitFilter(environment)

async function getWeather (city) {
  // This can be any async operation, like fetching data
  // from an external API, or getting records from a database.
  return request('weather.com', { city })
}
```

Use an `async` function:

```nunjucks
The weather in Toronto is {{ getWeather('Toronto') | await }}
```

That's all there is! `getWeather` will resolve and the result will show in the rendered template.

## How it works

It's actually very small, here's almost all of the code:

```js
async function awaitFilter (functionPromise, callback) {
  try {
    // The called function returns a Promise, which we
    // now `await` until its done
    const result = await functionPromise

    // Then we call the Nunjucks async filter callback
    callback(null, result)
  } catch (error) {
    // And if the `functionPromise` throws an error
    // Nunjucks will pick it up here
    callback(error)
  }
}
```

Using [Nunjucks Async Filters](https://mozilla.github.io/nunjucks/api.html#addfilter), this brings the `await` keyword to Nunjucks templates.
