# Authorisation

Broadly speaking, there are two forms of authorisation being performed in an API

- Endpoint authorisation
- Resource authorisation

# Endpoint Authorisation

Endpoint authorisation is probably the simplest form. 

- Blocks requests before they can enter an API endpoint.
- Generally done via .NET policies and roles
- At it's simplest, only needs to reference the claims in the user's id token.
- More complicated, requires access to the database to retrieve role/group information for the user.

Resource authorisation is the hardest, and the rest of this article pertains to it.

# Resource Authorisation

## How authorisation relationships are modelled

### Direct relationship model

- In this example, a `Business` has a single owner.
- This is modelled as a simple foreign key from a `Business` to a single `User`
- Assume that only the owner can perform read and write operations on the business.
- In this case, we need to check that the id of the user making the request is the same as the id of the owner of the business that the operation is being performed on.

```
class Business 
{
			public Id Guid { get; set; }
			public Guid OwnerId { get; set; }
			public User Owner { get; set; }
}
class User 
{
			public Id Guid { get; set; }
}
```



### Transitive relationship model

- In this example, a `Business` has many users.
- This user relationship is modelled via a `BusinessUser`  basic mapping table. 
- Assume that any of these users can perform read and write operations on the business.
- The business might have 1 user, 10 users, 100 users, 1000 users, etc
- In this case, we need to check the `BusinessUser` relationships for an organisation to check if the requesting user id has a relationship to it.
- Transitive relationships are more complicated than a direct relationship, because how we can perform the check in multiple ways:
  - - 

```
class Business 
{
			public Id Guid { get; set; }
			public ICollection<BusinessUser> Users { get; set; }
}
class BusinessUser 
{
			public BusinessId Guid { get; set; }
			public Business Business { get; set; }
			public UserId Guid { get; set; }
			public User User { get; set; }
}
class User 
{
			public Id Guid { get; set; }
}
```



## Ways to do the authorisation

These approaches consider the `Business` examples above.

### Approach 1 - Fetch all data first

- Pull all the data required to perform the authorisation check from the database first
- Perform the authorisation check
- Perform the operation if authorised

#### Advantages

- Can use framework tooling such as .NET policies and `IAuthorizationHandler`
- In some ways, more straight forward because the operations on the resources and the authorisations to perform those operations are separate.
- Are able to determine precisely why the authorisation failed

#### Disadvantages

- Requires bringing back the data from the database first.
  - Might be ok for the direct relationship model, as only returning a single entity
  - For the transitive relationship model, can result in huge data sets being returned
    - For example, if a `Business` had 1000s of users, you have to pull back 1000s of `BusinessUser` records.
- In the case of write operations, you usually don't need that data in memory.
- In the case of read operations, you may be bring back more data than needed for the operation itself.
- Database operation

---

### Approach 2 - Multiple queries

- Perform a database query to determine the outcome of the authorisation check.
- Perform the operation if authorised

#### WRITE EXAMPLE - Transitive Relationship (2 queries)

- We don't have to pull the `Business` back for this.
- We can simply query the `BusinessUser` table by `BusinessId` and `UserId`
  - If one result remains,  we know the user has access to this business.
- If auth passes, the write operation is executed as a second query.

#### Advantages

- Simpler queries
- Know exactly that an authorisation failed and why

#### Disadvantages

- More requests to the database

---

### Approach 3 - Authorisation integrated into the operation

- Add authorisation predicates to the database query that performs the operation.
- The number of rows affected gives insight into the outcome of the operation.
- If rows affected is zero, then the operation was not able to perform.

#### Advantages

- Single database call, improved performance

#### Disadvantages

- A zero rows affected result doesn't give exact information about why the authorisation failed.
  - It could be that the entity / entities being operated on were not found
  - It could be other query predicates that cause the result to have no rows affected.
- Queries for CRUD operations become more complicated

#### When to use this approach

- When you don't need to know why an authorisation operation failed

#### READ/WRITE EXAMPLE - Direct Relationship (1 query)

- Imagine a user wants to perform CRUD on a `Business` in the database.
- Query the `Business` table filtering by `Id` and `UserId`
- If one row is affected, the user is the owner.
- If zero rows is affected, either:
  - The user is not the owner
  - The business does not exist

#### READ EXAMPLE - Transitive Relationship (1 query)

- Imagine a user wants to retrieve a `Business` from the database.
- Query the `Business` table by id, left join on `BusinessUser` and then filter by `UserId`
- If one row is affected, the user is the owner.
- If zero rows is affected, either:
  - The user is not the owner
  - The business does not exist

## Security by obscurity

- When an operation is not authorised, by design of HTTP we can return a 403 Forbidden response.
- However, this response code leaks sensitive details about the system to leak out.
- In most cases, it's best to return a 400 Bad Request with a "not found" error code of some variety.
  - In an API situation, returning a `NotFound (404)` result directly isn't appropriately, because that is reserved for if the actual API endpoint is not found.
- This helps to prevent attackers from determining if resource IDs actually exist or not.
- With good authorisation, this is not an attack vector in itself, but it's best to reduce the surface area of an attack by not leaking their existence.

### An exception

- For search type endpoints, where a specific ID is not being used in the request, it's fine to just return an empty collection of results.
- Access control operations should be in place as part of the search query anyway.

### The concept of "Not Found" inside a non public API

- Private vs Public
  - By public I mean a request that you don't have to be signed in to make
  - By private I mean a request that you have to be signed in to make
- To preface, for public APIs, such as those that drive content driven apps/sites such as Contentful, the concept of "not found" is perfectly fine.
  - If that something that was once public, is no longer available, then it's appropriate to say it's "not found".
- However, a private API request that results in the concept of some data not being "found" has the following possibilities:
  - A user is prodding the API with random (or stolen) ids for entities that they don't have access to.
  - An API client application has bugs and is sending the wrong values/ids to an API endpoint
  - A user is trying to access data they once had access to, but is now deleted.
- From a security perspective, the worst case should always be assumed.
- If you have an endpoint that operates on a certain entity, and that entity is not found, then it should by default be considered an authorisation exception.
- In most cases, it should not be possible for an API Client app to be sending requests for data that:
  - Does not exist at all.
  - Does exist, but the requesting user does not have access to it.
- This matters mostly _inside_ the api. Code execution should stop with an authorisation exception when this situation occurs.
- However, as discussed in Security by Obscurity, it's always best for the API to respond to these kinds of unauthorised requests with a "not found" code.
- With all that said, you may be able to see now how the concept of data "not being found", and "data being found but you aren't allowed to touch it" as being two peas in the same pod. Either way, you're unauthorised to make that kind of request.
  - You're not allowed to access data that was deleted.
  - You're not allowed to prod the API with random IDs
- So in summary, in normal circumstances an API should not be getting calls by users for data that they don't have access to.
- By taking a hard line on this, we can safely always throw from our code an Authorisation exception when data is not available to a given user because:
  - The data isn't theirs
  - The data doesn't exist
  - In both scenarios, the request should never have been made legitimately.
- 

## Conclusion

- There is no one size fits all solution for authorisation
- Choosing the appropriate approach includes a number of trade offs including:
  - How performant the request should be.
  - How complicated the database queries should be.
  - Whether you need to know the specifics as to why an authorisation failed.

### Rough guide

- For higher performance, choose an integrated query, at the expense of:
  - Not knowing whether a record was not found or unauthorised
  - A more complicated query
- For less complicated database queries, choose a multi query approach at the expense of:
  - Performance (more trips to the database)
- In most situations, pulling all the data back from the database is probably the least preferred option unless:
  - You need the data any as part of the operation (mainly for reads)
  - The authorisation is modelled as a direct relationship
  - The authorisation is modelled as a transitive relationship, but the number of records in that relationship are bounded and minimal.