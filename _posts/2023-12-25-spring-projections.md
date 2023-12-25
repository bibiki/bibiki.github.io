---
layout: post
title: Spring Projections and Native Queries
---

I was involved a while back in an ongoing project that was being built with Java, Spring, Spring Data, and the related technologies. It was in this particular object that I first got to use projections.

To make sure that you know, projections are a pretty cool way to write your data transfer objects. I learned that Spring supports two ways to make use of projections. One, you may define an interface where you declare methods that return the data you need your data transfer object to contain. To teach Spring to map results from database queries onto projections, we annotate the methods inside those interfaces with @Value and pass in to @Value a spel string to specify what result set column should go the annotated field. Here is a short example taken from https://docs.spring.io:

```
interface NamesOnly {

    @Value("#{target.firstname + ' ' + target.lastname}")
    String getFullName();
}
```

The other way to use projections is to define actual classes, instead of interfaces. What I am about to describe here ends by making use of this kind of projections.

I found projections convenient to work with. The advantage of using them seemed to be that one need not write mappers to map an entity to a data transfer object when using them. Or rather, the mapping code is more convenient to write using the @Value annotation than writing a new class to contain mapper methods.

However, Spring does not know how to map the result set of a native query onto a projection interface as easily as it knows to do that with Spring Data queries. Unlike with projections, a CRUDRepository provided by Spring Data will know how to return instances of an entity class (one annotated with @Table) from a method that performs a native query. So, when the need arose to write a native query and return objects with fewer fields than the actual entity, projections were not as usuable as they would have been if the query was not a native one. As a result, the team had decided to introduce another entity object and configure it to map onto the same table as the original entity that the projection was to be used for. In other words, there were two entity classes configured with the same @Table(name = "table_name").

Now, of course, this did work, but it was a little confusing when reading the code and it was not immediately clear why there were two classes for the same entity. But as I continued to work in the same part of the code, I was facing a problem that was best solved with a native query and a new projection and this led me to understand the root problem that led to the code base having two different classes for the same entity. I wrote my native query, tried to use a projection as a return type of my method, and it did not work. I was faced with the dilemma: do I write a third entity class for the same table (LOOOOL), or do I use plain old data transfer objects along with mappers to-and-from the original entity? I opted to withhold my decision and rather do neither. I wanted to see if there actually is a way to use projections with native queries.

It turns out, there is.

There are two possible scenarios: one, you need a single native query, or two, you need multiple different native queries against a particular table. In both cases, we use annotations in our entity class to specify native queries.

Let's say that our entity class is named Users and we need a single native query against our users table. Then here is a sample configuration of our Users.java class to make sure that the native query's result is properly mapped onto our projection:

```
@NamedNativeQuery(name = "Users.myNativeQueryMethod",
query = "your select query",
resultSetMapping = "sqlResultMappingName")
@SqlResultSetMapping(name = "sqlResultMappingName",
   classes = @ConstructorResult(targetClass = UserProjection.class,
                                columns = {@ColumnResult(name = "userId", type = Integer.class),
                                        @ColumnResult(name = "lastName", type = String.class),
                                        @ColumnResult(name = "firstName", type = String.class),
                                        @ColumnResult(name = "createdOn", type = Date.class)}))
@Table(name = "users")
public class Users {

}
```

In addition to that, you will have a repository class that interacts with your users table, and it will contain your method that performs the native query. Here is a sample of such a class:

```
public interface UserRepository extends PagingAndSortingRepository<Users, Long> {

        @Query(nativeQuery = true)
        List<UserProjection> myNativeQueryMethod(@Param("param") Sring someParam);
}
```

Finally, this example assumes that you have a class UserProjection.java that looks like this:

```
public class UserProjection {

        Integer userId;
        String lastName;
        String firstName;
        Date createdOn;

    public class UserProjection(Integer userId, String lastName, String firstName, Date createdOn) {
        this.userId = userId;
        this.lastName = lastName;
        this.firstName = firstName;
        this.createdOn = createdOn;
    }
}
```

This works fine for when you have a single native query that you run against your users table. If, however, you have more than one native query, then you need to wrap your multiple @NamedNativeQuery annotations inside a single @NamedNativeQueries annotation in your Users.java class.

And that's a wrap. I wanted to document this problem-solution pair here for my later use, or simply to re-enforce my memory of for later use.e
