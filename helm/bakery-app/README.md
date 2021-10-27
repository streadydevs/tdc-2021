# Bakery app

This is a sample service serving cakes over http port 8080

The cake is from [asciiart.website](https://asciiart.website/index.php?art=events/birthday)

## Running

```
go mod tidy
go run .
```

## Accessing the server
```
curl localhost:8080
```

## Notes
* The server has a health check endpoint at `/healthz`, all other endpoints serve cakes
