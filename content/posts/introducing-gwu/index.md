+++
title = 'Generic Web Handlers in Go'
date = 2024-07-10T22:22:00+01:00
published = false
tags = ['Software Engineering', 'Go', 'Web Development', 'Gwu']
+++

> The idea stems from a very interesting [Reddit thread](https://www.reddit.com/r/golang/comments/1dxat13/whats_a_really_good_blog_post_youve_read_lately/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button) which lead me to [Willem Schots' blog post](https://www.willem.dev/articles/generic-http-handlers/), giving me a starting point for further advancing the idea of generic web handlers.

> Heads up: This post and implementation for generic web handlers focuses on APIs (with JSON). There is no other data-interchange format, HTML rendering or templating involved. However, there should be no issue in extending the idea for further use cases, and I encourage you to do so. Please share your ideas and implementations with me! :)

## Web Handlers in Go
I'll keep this simple, as I expect you to be familiar with the basics of Go and `net/http` already.
Web handlers take an incoming HTTP request, perform some action, and write an HTTP response.

This "action" often involves some CRUD:
- *Create* an entity: Create a peom in a peom store.
- *Read* an entity: List all poems from that store.
- *Update* an entity: Update a poem in the store.
- *Delete* an entity: Delete a certain poem from that store.

For our `Create an entity` example, we might have a handler like this:
```go
mux.HandleFunc("POST /order", func CreateOrder(w http.ResponseWriter, r *http.Request) {
    // Parse incoming JSON payload
    var order Order
    if err := json.NewDecoder(r.Body).Decode(&order); err != nil { ... }

    // Save order to database
    if err := db.SaveOrder(order); err != nil { ... }

    // Respond withcreated order
    w.Header().Set("Content-Type", "application/json")
    w.WriteHeader(http.StatusCreated)
    json.NewEncoder(w).Encode(order)
})
```

## The Beauty of the Idea
You might notice a potential pattern here:
1. Data comes in
2. Action is performed
3. Data goes out

![Illustration of the pattern](images/beauty-of-the-idea.webp)
 
This pattern will most likely be repeated in every handler, with only the specifics changing. Have you noticed it in your own code bases, maybe even written a helper function to reduce boilerplate?

The greatest thing: If we know that the output is always JSON, and the business logic can be arbitrary, same as the input, as long as it's processed to the business logic's satisfaction, we can abstract this pattern into a generic handler.

The very abstract pseudocode for this generic handler might look something like this:
```pseudo
function handle(r Request) -> Response {
    in = FromRequest[Poem](request)
    
    out = BusinessLogic[Poem](in)
    
    return IntoResponse(out)
}
```

A basic, working Go implementation of this pseudocode can be found in [Willem Schots' blog post](https://www.willem.dev/articles/generic-http-handlers/). However, I wanted to explore this idea further, so let's go. 

## Upfront Considerations

## Generic Web Handlers

### Implementation

### Demo

## Gwu Is Born!

### The Idea

### What Does the Future Hold?

## Discussion: Is This Too Complex?
- idk, maybe yes? lmk!

## Conclusion
- Don't take all of this too serious, if you like it, give it a spin. If not, ignore it, or better: tell me why!
- Gwu is expected to grow in the future, I'll try to put my personal thoughts and best practices there, while experimenting with smart abstractions that integrate well in std compliant environment.