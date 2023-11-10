# Go Expert - Web Server (APIs)

Main used libraries
- Go Chi: chi is a lightweight, idiomatic and composable router for building Go HTTP services.
- Swag: Swag converts Go annotations to Swagger Documentation 2.0.
- Viper: Viper is a complete configuration solution for Go applications.
- Gorm: An ORM library for Golang which aims to be developer friendly.
- JWTAuth: A middleware package provides a simple way to verify a JWT token from a http request and send the result down the request context.

Swagger - OpenAPI 2.0 documentation
- Installing swagger: https://github.com/swaggo/http-swagger
- Declarative Comments Format: https://github.com/swaggo/swag#declarative-comments-format
- Run the command below to generate the documentation files. When you run the command below, it will create a folder `docs` with three files regarding the documentation. It creates all the three files in this folder (.go, .json, and .yaml).
```
swag init -g cmd/server/main/go
```
- The endpoint below exposes the documentation.
```
import {
	_ "github.com/andrewsjuchem/go-expert-web-server/docs"
  httpSwagger "github.com/swaggo/http-swagger"
}
...
r.Get("/docs/*", httpSwagger.Handler(httpSwagger.URL("http://localhost:8000/docs/doc.json")))
...
```

Running the server
```
cd cmd/server && go run main.go
```

Options to test the endpoints
- Browse to the "test" folder and call the endpoints by pressing "send request" from within the http files (through VSCode).
- Use OpenAPI: http://localhost:8000/docs/index.html#/
- Call them from a thrid part tool such as Postman.

Object creation flow at development stage
1. Entities
2. Database Interfaces
3. Entity Handlers

User objects flow
1. User handler receives the request and converts the input JSON into a User DTO.
2. User DTO values are used to create an user through the User Entity.
3. The User Entity object is sent to the User DB interface to create the user in the database.

Authentication
- The jwt secret in a config (.env file).
- A "jwtauth" object (with the secret) is added to each request through a middleware.
- When the endpoint to get/generate the token is called, it uses this "jwtauth" object from the request to create the token and return the token to the user.
- The user will use this token and send to subsequent request as a bearer token.
- In every other request, the bearer token gets validated through Chi's middlewares (jwtauth.Verifier + jwtauth.Authenticator).
- Since this app both generates the authentication token and uses it to authenticate the user, the secret is only in one place (in the config file).
- If the "GetJWT" function was in another app, that other app would need to have the same secret in order to create a valid token.
