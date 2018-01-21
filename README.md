ts-framework
============

[![pipeline status](https://gitlab.devnup.com/npm/ts-framework/badges/master/pipeline.svg)](https://gitlab.devnup.com/npm/ts-framework/commits/master)
[![coverage report](https://gitlab.devnup.com/npm/ts-framework/badges/master/coverage.svg)](https://gitlab.devnup.com/npm/ts-framework/commits/master)

A minimalistic framework for typescript based applications, with async/await and decorators support.

## Getting Started

### Installing the Alpha Preview

The currently API is considered to be "alpha" so it can change at any time. To 
ensure your project won't crash, refer to the specific commit, instead of a 
semver tag or git branch.

For example:

```bash
# Install using Yarn
yarn add git+https://gitlab.devnup.com/npm/ts-framework.git#GIT_REV_HASH 

# Install using NPM
npm install --save git+https://gitlab.devnup.com/npm/ts-framework.git#GIT_REV_HASH 
``` 

### TL;DR - A single file server

Configure a new Server instance and start listening on desired port. 

```typescript
import Server, { Logger, BaseRequest, BaseResponse } from 'ts-framework';

// Define the server configuration
const server = new Server({
  port: process.env.PORT as any || 3000,
  routes: {
    get: {
      '/': async (req: BaseRequest, res: BaseResponse) => {
        res.success({ message: 'Hello world!' })
      }
    }
  },
});


// Startup the simple server
server.listen()
  .then(() => Logger.info(`Server listening on port: ${server.config.port}`))
  .catch(error => {
    console.error(error);
    process.exit(1);
  });
```

### Usage Guide

Follow the [Usage Guide](./GUIDE.md) for the basic boilerplate and a sample project configuration with
Database and user authentication samples.


## Documentation

It's in the roadmap an automated documentation based on JSDocs. Currently there are only the JSDocs tags inside of some 
key class and components.

Internal components:

- Logger
- Database
- Router
  - Controllers
  - Filters
  - Responses

External components already available as middlewares: 

- OAuth 2.0 (express-oauth2-server)
- Sentry (RavenJS)
- CORS (express/cors)
- Multipart (express/multer)
- User Agent (express-useragent and request-ip)
- Body Parser (express/body-parser)
- Method Override (express/method-override)
- Cookie Parser (express/cookie-parser)

Other external plugins and middlewares for the framework

- **[ts-framework-versioning](https://gitlab.devnup.com/npm/ts-framework-versioning)**

    Handles API versioning using HTTP Headers.
    
- **[ts-framework-notification](https://gitlab.devnup.com/npm/ts-framework-notification)**

    Handles transactional notifications using E-mails with templates, and in the future Push notifications.

- **[ts-framework-migration](https://gitlab.devnup.com/npm/ts-framework-migration)**

    Advanced usage plugin for handling Schema migrations safely within production environments.



### Server

The basic HTTP Server definitions, wraps an Express to integrate all framework modules.


#### Utility classes

- **Logger**: A simple Logger based on [Winston](https://npmjs.org/package/winston).

#### Base classes

- **BaseError**: The base error instance for all framework exceptions.
    - `error.stackId`: An unique uuid/v4 for Errors to be sent to external service, such as ELK or Sentry.
    - `error.details`: An object with misc details associated with the error instance. 

- **BaseRequest**: Extends Express.js request class for framework binding.
    - `request.logger`: The Server logger instance, if supplied in the Server constructor.

- **BaseResponse**: Extends Express.js response class for framework binding.
    - `response.success(data: any)`: A shortcut for setting status as `200` and calling `data.toJSON()` if available.
    - `response.error(error: String | Error)`: Handles errors before sending to response, cleaning the stack and assigned an unique `stackId`.

#### Decorators

- **@Controller(baseRoute: string, middlewares?: Function[])**: Decorator for controller classes.

- **@Get(route: string, middlewares?: Function[])**: Decorator for GET methods, must be static.

- **@Post(route: string, middlewares?: Function[])**: Decorator for POST methods, must be static.

- **@Put(route: string, middlewares?: Function[])**: Decorator for PUT methods, must be static.

- **@Delete(route: string, middlewares?: Function[])**: Decorator for DELETE methods, must be static.


### Database

The MongoDB ODM definitions.

#### Base classes

- **BaseModel**: The base class for all model instances decorated with `@Model`. 
    - `BaseModel.modelName`: The name of the model collection in MongoDB, as supplied in the `@Model` decorator.


#### Decorators

- **@Model(name: string)**: Assigns a name for the decorated model class to be used as collection name in MongoDB. Must
be used in classes that extends `BaseModel`.


## Full module reference

To generate the Typedoc reference of the available modules, run the following command:

```sh
yarn run docs
```

Then check the documentation at `./docs/index.html`.


## License

The project is licensed under the [MIT License](./LICENSE.md).