# apollo-schema-extend

Extends your Apollo Server Express based graphql server with an external graphql source.
Merges the external schema into the local schema and forwards any (parts of) request to be resolved by the external source.
This library is intended for situations where Apollo Federation doesn't provide sufficient control or is not an option for other reasons.

##Basic usage:
In this example `myExternalSchema` is a `GraphQLSchema` retrieved by using an `IntrospectionQuery` and `GqlDataSource` is a derived class of `apollo-datasource-graphql`.

```typescript
const withMyExternalSchema = withExternalSchema(myExternalSchema, {
  dataSource: { name: 'external', factory: () => new GqlDataSource('https://myExternal.com/graphql') },
})

const apolloServer = new ApolloServer(withMyExternalSchema(localApolloServerConfig))
```

##Recommended data source:

```typescript
import { GraphQLDataSource } from 'apollo-datasource-graphql'
import { Context } from '...'

export class GqlDataSource extends GraphQLDataSource<Context> {
  constructor(baseUrl: string) {
    super()
    this.baseURL = baseUrl
  }

  protected willSendRequest(request: any) {
    // Add authorization header
  }
}
```
