# @parry-gg/client

A TypeScript/JavaScript client library for Parry.gg's gRPC services, providing access to tournament management APIs. This client is generated from Protocol Buffer definitions using Google's [protoc](https://developers.google.com/protocol-buffers/docs/reference/overview) compiler with the gRPC-Web plugin.

## Installation

```bash
npm install @parry-gg/client google-protobuf grpc-web
```

### Peer Dependencies

This package requires the following peer dependencies:

- `google-protobuf`: Protocol Buffer runtime for JavaScript
- `grpc-web`: gRPC-Web client library  
- `xhr2`: XMLHttpRequest polyfill (optional, for Node.js environments)

## Environment Setup

### Node.js

For Node.js environments, you need to provide an XMLHttpRequest polyfill:

```javascript
const xhr = require('xhr2');
global.XMLHttpRequest = xhr;
```

Or using ES modules:

```javascript
import xhr from 'xhr2';
(global as any).XMLHttpRequest = xhr;
```

## Authentication

Before using the client, you need to:

1. Create an API key at [https://parry.gg](https://parry.gg).
2. Include the API key in request headers as `X-API-KEY`

## Basic Usage

```typescript
import { UserServiceClient, UpdateUserRequest, MutableUser } from '@parry-gg/client';

// Initialize client
const client = new UserServiceClient('https://grpcweb.parry.gg');

// Create request
const request = new UpdateUserRequest();
request.setId('user-id-here');

const user = new MutableUser();
user.setGamerTag('MyGamerTag');
user.setFirstName('John');
user.setLastName('Doe');
request.setUser(user);

// Make API call
const response = await client.updateUser(request, {
  'X-API-KEY': 'your-api-key-here'
});
```

## Available Services

The client provides access to the following gRPC services:

- **UserServiceClient**: User management operations
- **TournamentServiceClient**: Tournament operations
- **EventServiceClient**: Event management
- **BracketServiceClient**: Bracket operations
- **PhaseServiceClient**: Phase management
- **EntrantServiceClient**: Entrant operations
- **GameServiceClient**: Game data
- **HierarchyServiceClient**: Tournament hierarchy
- **MatchServiceClient**: Match operations
- **NotificationServiceClient**: Notification system
- **PageContentServiceClient**: Content management

## Data Models

The package includes TypeScript definitions for all data models:

- **User**: User profiles and information
- **Tournament**: Tournament data and metadata
- **Event**: Event details and configuration
- **Bracket**: Bracket structure and progression
- **Phase**: Tournament phases
- **Entrant**: Participant information
- **Game**: Game definitions
- **Match**: Match data and results
- **Notification**: System notifications
- **Image**: Image metadata
- **Filter**: Query filters
- **Slug**: URL slugs
- **Request**: Common request types

## Configuration

## Error Handling

The client throws gRPC-Web errors that can be caught and handled. Error codes follow the [gRPC status code conventions](https://grpc.io/docs/guides/status-codes/):

```typescript
try {
  const response = await client.someMethod(request, metadata);
} catch (error) {
  if (error.code === StatusCode.UNAUTHENTICATED) {
    console.error('Invalid API key');
  } else if (error.code === StatusCode.NOT_FOUND) {
    console.error('Resource not found');
  } else {
    console.error('API error:', error.message);
  }
}
```

## TypeScript Support

The package includes full TypeScript definitions. All models and services are strongly typed:

```typescript
import { User, MutableUser, UserServiceClient } from '@parry-gg/client';

const user: User = response.getUser();
const gamerTag: string = user.getGamerTag();
```

## License

MIT
