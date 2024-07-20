+++
title = 'Generic Web Handlers in Go'
date = 2024-07-20T22:22:00+02:00
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
    var order Order
    if err := json.NewDecoder(r.Body).Decode(&order); err != nil { ... }

    if err := db.SaveOrder(order); err != nil { ... }

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
The point where I immediately deviated from Schots' implementation was his definition of the generic handler function:
```go
// TargetFunc executes the business logic and transforms Input to Output.
type TargetFunc[In any, Out any] func(context.Context, In) (Out, error)

// Handle returns an http.HandlerFunc that executes the business logic using the provided TargetFunc.
func Handle[In any, Out any](f TargetFunc[In, Out]) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) { ... })
}
```

What about the HTTP status codes? This question struck my mind. In Schots' example, the `Handle` func writes `http.StatusBadRequest` (400) to the response when the provided `TargetFunc` returns an error. However, I would highly doubt that business logic only errors with bad requests, what about forbidden (403), not found (404), internal server error (500), ...?

This might seem trivial but what's indicated here isn't: The executed `TargetFunc` is not service logic, it leaks to the outside and is therefore still `Handler` or `Controller` logic. Therefore, I also disagree with Schots' naming of `service.go` and `Service.CreateNote`/`Service.UpdateNote`. 

A `TargetFunc` must be aware of its context: The errors it returns are visible to client, and it might indicate the HTTP status code which is written to client on failure. Keep this in mind for later, when we define our own function signature.

Furthermore, Schots' does not provide a way to modify the `In`. He states this himself:
> If you’re dealing with complex requests you could inject some kind of generic constructor function into the Handle function: `type ConstructorFunc[In any](r *http.Request)(In, error)`

This is not only important for complex requests but absolutely necessary if you provide certain info (like an ID) to your endpoint via query parameters. It can also be useful for validations, but we'll inspect that in more detail later.

Also, Schots' misses, IMHO, some extra options to pass to a handler, like a logger. I'll touch on that later, again. ;) 

## Generic Web Handlers

Now, with fundamentals and considerations out of the way, let's start to implement our own version of generic web handlers.

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