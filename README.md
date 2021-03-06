# @okgrow/graphql-scalars

[![npm version](https://badge.fury.io/js/%40okgrow%2Fgraphql-scalars.svg)](https://badge.fury.io/js/%40okgrow%2Fgraphql-scalars)
[![Build Status](https://semaphoreci.com/api/v1/projects/649ab71f-35fe-440e-8e4b-3f68aaad0f2a/1545482/shields_badge.svg)](https://semaphoreci.com/okgrow/graphql-scalars)

> A library of custom GraphQL [scalar types](http://graphql.org/learn/schema/#scalar-types) for creating precise type-safe GraphQL schemas.

## Installation
```
npm install --save @okgrow/graphql-scalars
```


## Usage
To use these scalars you'll need to add them in two places, your schema and your resolvers map.

In your schema:
```graphql
scalar DateTime

scalar NonPositiveInt
scalar PositiveInt
scalar NonNegativeInt
scalar NegativeInt

scalar NonPositiveFloat
scalar PositiveFloat
scalar NonNegativeFloat
scalar NegativeFloat

scalar EmailAddress
scalar URL

scalar PhoneNumber
scalar PostalCode
```

In your resolver map, first import them:
```js
import {
  DateTime,

  NonPositiveInt,
  PositiveInt,
  NonNegativeInt,
  NegativeInt,

  NonPositiveFloat,
  PositiveFloat,
  NonNegativeFloat,
  NegativeFloat,

  EmailAddress,
  URL,

  PhoneNumber,
  PostalCode,
} from '@okgrow/graphql-scalars';
```

Then make sure they're in the root resolver map like this:

```js
const myResolverMap = {
  DateTime,

  NonPositiveInt,
  PositiveInt,
  NonNegativeInt,
  NegativeInt,

  NonPositiveFloat,
  PositiveFloat,
  NonNegativeFloat,
  NegativeFloat,

  EmailAddress,
  URL,

  PhoneNumber,
  PostalCode,

  Query: {
    // more stuff here
  },

  Mutation: {
    // more stuff here
  },
}
```

Alternatively, use the default import and ES6's spread operator syntax:
```js
import OKGGraphQLScalars from '@okgrow/graphql-scalars';
```

Then make sure they're in the root resolver map like this:

```js
const myResolverMap = {
  ...OKGGraphQLScalars,

  Query: {
    // more stuff here
  },

  Mutation: {
    // more stuff here
  },
}
```


That's it. Now you can use these scalar types in your schema definition like this:
```graphql
type Person {
  birthDate: DateTime
  ageInYears: PositiveInt

  heightInInches: PositiveFloat

  minimumHourlyRate: NonNegativeFloat

  currentlyActiveProjects: NonNegativeInt

  email: EmailAddress
  homePage: URL

  phoneNumber: PhoneNumber
  homePostalCode: PostalCode
}

```

These scalars can be used just like the base, built-in ones.


## Why?
The primary purposes these scalars, really of _all_ types are to:

1. Communicate to users of your schema exactly what they can expect or to at least _reduce_
ambiguity in cases where that's possible. For example if you have a `Person` type in your schema
and that type has as field like `ageInYears`, the value of that can only be null or a positive
integer (or float, depending on how you want your schema to work). It should never be zero or
negative.
1. Run-time type checking. GraphQL helps to tighten up the contract between client and server. It
does this with strong typing of the _interface_ (or _schema_). This helps us have greater
confidence about what we're receiving from the server and what the server is receiving from the
client.

This package adds to the base options available in GraphQL to support types that are reasonably
common in defining schemas or interfaces to data.


## The Types

### DateTime
Use real JavaScript Dates for GraphQL fields. Currently you can use a String or an Int (e.g., a
timestamp in milliseconds) to represent a date/time. This scalar makes it easy to be explicit about
the type and have a real JavaScript Date returned that the client can use _without_ doing the
inevitable parsing or conversion themselves.

### NonNegativeInt
Integers that will have a value of 0 or more. Uses [`parseInt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt).

### NonPositiveInt
Integers that will have a value of 0 or less. Uses [`parseInt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt).

### PositiveInt
Integers that will have a value greater than 0. Uses [`parseInt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt).

### NegativeInt
Integers that will have a value less than 0. Uses [`parseInt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt).

### NonNegativeFloat
Floats that will have a value of 0 or more. Uses [`parseFloat()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseFloat).

### NonPositiveFloat
Floats that will have a value of 0 or less. Uses [`parseFloat()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseFloat).

### PositiveFloat
Floats that will have a value greater than 0. Uses [`parseFloat()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseFloat).

### NegativeFloat
Floats that will have a value less than 0. Uses [`parseFloat()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseFloat).

### EmailAddress
A field whose value conforms to the standard internet email address format as specified in
[RFC822](https://www.w3.org/Protocols/rfc822/).

### URL
A field whose value conforms to the standard URL format as specified in
[RFC3986](https://www.ietf.org/rfc/rfc3986.txt).

### PhoneNumber
A field whose value conforms to the standard E.164 format as specified in
[E.164 specification](https://en.wikipedia.org/wiki/E.164). Basically this is `+17895551234`.
The very powerful
[`libphonenumber` library](https://github.com/googlei18n/libphonenumber) is available to take
_that_ format, parse and display it in whatever display format you want. It can also be used to
parse user input and _get_ the E.164 format to pass _into_ a schema.

### PostalCode
We're going to start with a limited set as suggested [here] (http://www.pixelenvision.com/1708/zip-postal-code-validation-regex-php-code-for-12-countries/)
and [here] (https://stackoverflow.com/questions/578406/what-is-the-ultimate-postal-code-and-zip-regex).

Which gives us the following countries:

- US - United States
- UK - United Kingdom
- DE - Germany
- CA - Canada
- FR - France
- IT - Italy
- AU - Australia
- NL - Netherlands
- ES - Spain
- DK - Denmark
- SE - Sweden
- BE - Belgium
- IN - India

This is really a practical decision of weight (of the package) vs. completeness.

In the future we might expand this list and use the more comprehensive list found [here] (http://unicode.org/cldr/trac/browser/tags/release-26-0-1/common/supplemental/postalCodeData.xml).


## Future
We'd like to keep growing this package, within reason, to include the scalar types that are widely
required when defining GraphQL schemas. We welcome both suggestions and pull requests. One idea
we're considering is:

- BLOB, could be could be a base64-encoded object of some kind

## What's this all about?
GraphQL is a wonderful new approach to application data and API layers that's gaining momentum. If
you have not heard of it, start [here](http://graphql.org/learn/) and check out
[Apollo](http://dev.apollodata.com/) also.

However, for all of GraphQL's greatness. It is missing a couple things that we have (and you might)
find very useful in defining your schemas. Namely GraphQL has a
[limited set of scalar types](http://graphql.org/learn/schema/#scalar-types) and we have found there
are some additional scalar types that are useful in being more precise in our schemas. Thankfully,
those sharp GraphQL folks provided a simple way to add new custom scalar types if needed. That's
what this package does.

**NOTE:** We don't fault the GraphQL folks for these omissions. They have kept the core small and
clean. Arguably not every project needs these additional scalar types. But _we_ have, and now _you_
can use them too if needed.


## License
Released under the [MIT license](https://github.com/okgrow/analytics/blob/master/License.md).


## Contributing
Issues and Pull Requests are always welcome.

Please read our [contribution guidelines](https://okgrow.github.io/guides/docs/open-source-contributing.html).
