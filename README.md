# gqlgen-relay
GraphQL Relay support for gqlgen

## Usage
1. Add relay
```bash
go get -u gitlab.com/hookactions/gqlgen-relay
```

2. Add to code

2a. Go code
```go
//go:generate sh -c "go run gitlab.com/hookactions/gqlgen-relay -pkg model -name User -type *User -cursor > user_relay.go"
package model

type User struct {
	FirstName string 
    LastName string
}
```

2b. schema.graphql

```graphql
interface Node {
    id: ID!
}

type PageInfo {
    hasNextPage: Boolean!
    hasPreviousPage: Boolean!
    startCursor: String
    endCursor: String
}

type User {
    firstName: String!
    lastName: String!
}

type UserEdge {
    node: User
    cursor: String
}

type UserConnection {
    edges: [UserEdge]
    pageInfo: PageInfo!
    totalCount: Int
}
```

2c. gqlgen.yaml

```yaml
...
models:
  # existing config
  # ...
  # New
  PageInfo:
    model: gitlab.com/hookactions/gqlgen-relay/relay.PageInfo
  Node:
    model: gitlab.com/hookactions/gqlgen-relay/relay.Node
  User:
    model: gitlab.com/your/package/model.User
  UserEdge:
    model: gitlab.com/your/package/model.UserEdge
  UserConnection:
    model: gitlab.com/your/package/model.UserConnection
```
