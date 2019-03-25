# express-openapi-validator-example

## Usage

Setup: Install dependencies

```shell
# 1. clone this repo
git clone https://github.com/cdimascio/express-openapi-validator-example

# 2. install dependencies
npm install
```

Start the Api server

```shell
npm start
```

## Try it

Try the out the following requests.

The [express-openapi-validator](https://github.com/cdimascio/express-openapi-validator) automatically validates each request against an [openapi 3 specification](openapi.yaml). If a request is does not match the spec, [express-openapi-validator](https://github.com/cdimascio/express-openapi-validator) automatically returns an appropriate error response.

### Validate a query parameter with a value constraint

```shell
curl http://localhost:3000/v1/pets/as |jq
{
  "errors": [
    {
      "path": "id",
      "errorCode": "type.openapi.validation",
      "message": "should be integer",
      "location": "path"
    }
  ]
}
```

### Validate a query parameter with a range constraint

```shell
curl http://localhost:3000/v1/pets?limit=1 |jq
{
  "errors": [
    {
      "path": "limit",
      "errorCode": "minimum.openapi.validation",
      "message": "should be >= 5",
      "location": "query"
    },
    {
      "path": "test",
      "errorCode": "required.openapi.validation",
      "message": "should have required property 'test'",
      "location": "query"
    }
  ]
}
```

### Validate the query parameter's value type

```shell
curl --request POST \
  --url http://localhost:3000/v1/pets \
  --header 'content-type: application/xml' \
  --data '{
        "name": "test"
}' |jq
{
  "errors": [
    {
      "message": "Unsupported Content-Type application/xml"
    }
  ]
}
```

### Validate a POST body to ensure required parameters are present

```shell
λ  my-test curl --request POST \
  --url http://localhost:3000/v1/pets \
  --header 'content-type: application/json' \
  --data '{
}'|jq
  "errors": [
    {
      "path": "name",
      "errorCode": "required.openapi.validation",
      "message": "should have required property 'name'",
      "location": "body"
    }
  ]
}
```

### ...and much more. Try it out!

## Fetch the spec

curl http://localhost:3000/spec
