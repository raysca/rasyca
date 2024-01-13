---
title:  Better Fetch API with Decorators
date:   2023-10-10 17:52:34 +0100
---

## Introduction

Like most full stack JavaScript developers, I have come to love the web Fetch API. It is a simple and elegant way to make HTTP requests. However, the Fetch API lacks some features that are essential for making HTTP requests in a production environment. Here are some of those features:

- [Retry](https://github.com/raysca/super-fetch/blob/main/src/retry.ts)
- [Timeout](https://github.com/raysca/super-fetch/blob/main/src/timeout.ts)
- [Logging](https://github.com/raysca/super-fetch/blob/main/src/log.ts)
- [Headers](https://github.com/raysca/super-fetch/blob/main/src/headers.ts)

In this post, I will show how to use the decorator pattern to enhance the Fetch API and add some of these features without modifying the Fetch API itself.

## What is a decorator?

A decorator is a function that takes another function and extends the behavior of the latter function without explicitly modifying it. They key point is that a decorator function returns a modified version of the original function.

## Logging Decorator

The simplest decorator to implement is the logger decorator. It simply logs the url and the response of the http request. Here is the code:

```js

export const withLogger = (fetch, logger = console) => {
    return async (...args) => {
        const [input] = args;
        
        // This behaves exactly like the fetch api
        const response = await fetch(...args);

        // Check if the first input is a string or a URL and log accordingly
        if (typeof input === "string") {
            logger.info(`fetching ${input} and got ${response.status}`);
        }
        else if (input instanceof URL) {
            logger.info(`fetching ${input} and got ${response.status}`);
        }
        else {
            logger.info(`fetching ${input.url} and got ${response.status}`);
        }
        return response;
    }
}
```

Now we can use the `withLogger` decorator to log the url and the response of the http request.

```js
// This behaves exactly like the fetch api
const fetchWithLogger = withLogger(fetch);
// This will log the url and the response of the http request
fetchWithLogger("https://jsonplaceholder.typicode.com/todos/1");
```

## Retry Decorator

A more useful decorator is the `Retry Decorator`. It retries an http request if it fails. Here is the code:

```js

interface RetryOptions {
    /**
     * The number of times to retry the http request
     */
    retryCount: number

    /**
     * The delay between retries in milliseconds
     */
    retryDelay?: number
    
    /**
     * The http status codes to retry on
     */
    retryOn?: number[]
}

export const withRetry = (fetch: Fetch, options: RetryOptions) => {
    const { retryCount = 1, retryDelay = 1000, retryOn = [500] } = options;
    let response: Response;

    return async (...args: any): Promise<Response> => {
        let retry = 0;
        
        while (retry < retryCount) {
            // This behaves exactly like the fetch api
            response = await fetch(...args);

            // Check if the response status is in the retryOn array
            // If it is, then retry the http request
            if (retryOn.includes(response.status)) {
                retry++;
                // Wait for the retryDelay before retrying the http request
                await new Promise((resolve) => setTimeout(resolve, retryDelay));
            }
            else {
                return response;
            }
        }

        return response;
    }
}
```

Now we can use the `withRetry` decorator to retry the http request if it fails.

```js
// Create a new fetch function that retries the http request if it fails
const fetchWithRetry = withRetry(fetch, { retryCount: 3 });

// This will retry the http request if it fails
await fetchWithRetry("https://jsonplaceholder.typicode.com/todos/1");
```

## Combining Decorators

One of the advantages of using a decorator is that they can be combined together to even further extend the behavior of a function. This is called [composition](https://en.wikipedia.org/wiki/Function_composition#:~:text=In%20mathematics%2C%20function%20composition%20is,the%20function%20f%20to%20x.). Here is an example of how to use the `withLogger` and `withRetry` decorators together.

```js
// Create a new fetch function that retries the http request if it fails and logs the url and the response of the http request
const fetchWithRetryAndLogger = withLogger(withRetry(fetch, { retryCount: 3 }));

// This new fetch function will behave exactly like the fetch api
await fetchWithRetryAndLogger("https://jsonplaceholder.typicode.com/todos/1");
```

This [github repository](https://github.com/raysca/super-fetch) contains the full code for the decorators discussed in this post.

## Conclusion

The decorator pattern is a simple and elegant way to extend the behavior of a function without explicitly modifying it. It is a powerful tool in the hands of a developer and can be used to solve a lot of problems. In this post, I have shown how to use the decorator pattern to enhance the Fetch API and add some features that are essential for making http requests in a production environment.

## References

- [Decorator pattern](https://en.wikipedia.org/wiki/Decorator_pattern)
- [Function composition](https://en.wikipedia.org/wiki/Function_composition#:~:text=In%20mathematics%2C%20function%20composition%20is,the%20function%20f%20to%20x.)
- [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
