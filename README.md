# Unsplash API

Unsplash is an image sharing app. Users are able to search for high resolution, free-to-use images and download them for use.  
To use the Unsplash API, authentication is needed. You can acquire an access token through first logging in or creating an account *[here](https://unsplash.com/oauth/applications)*, then registering a new application.  
Documentation for the Unsplash API can be found *[here](https://unsplash.com/documentation)*.

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