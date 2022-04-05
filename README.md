# Unsplash API

**Unsplash** is an image sharing app. Users are able to search for high resolution, free-to-use images and download them for use. Today, we are going to work on displaying the first 10 photos posted on Unsplash for a query. The user is responsible for providing the query param.

Documentation for the Unsplash API can be found [here](https://unsplash.com/documentation).

***

Authentication is needed to display photo results for a query. You can acquire an access token through first logging in or creating an account [here](https://unsplash.com/oauth/applications), then registering a new application. Then, we can get the access token and save it as an environment variable.

![The application that was created to gain access to the access token](/img/application.png)

![The access token in the applications, blurred](/img/key.jpg)

***

To implement the Unsplash API in PHP...
```php
Route::get('/unsplash', function (Request $request) {
    $query = $request->query('query');

    $cacheKey = "unsplash-api-$query";

    $response = Cache::remember($cacheKey, 60, function () use ($query) {
        return Http::withHeaders([
            'Authorization' => "Client-ID " . env('UNSPLASH_API_KEY'),
        ])
        ->get("https://api.unsplash.com/search/photos?query=$query&orientation=landscape&page=1")
        ->object();
    });
    
    return view('api.unsplash', [
        'response' => $response,
    ]);
});
```

We are applying the params *query*, *orientation*, and *page*.

***

The response is a JSON object that looks like this:

![JSON object with search results for the keyword "city"](/img/json_results.png)

***

We can output the first 10 most relevant results to a page with the image, description of the image, and the date at which the image was uploaded to Unsplash. This is what the output will look like.

![A page outputting the top 10 search results for the keyword "city"](/img/city_results.jpg)