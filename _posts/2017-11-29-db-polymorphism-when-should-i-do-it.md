---
layout: post
title: "DB Polymorphism: When Should I do it?"
author: "Ming Xiang Chan"
date: 2017-11-28 10:06:11 +0800
tags: [postgresql, database]
preview: http://www.autanact.com/wp-content/uploads/2016/11/fme_formats.png
---

Polymorphism is the sharing of a common interface between different domains of data. We encounter it all the time in programming, some simple examples:

1. performing mathematical operations on *Floats* and *Integers*
2. React components all adhere to the *React component lifecycle* and posses `state` and `props`

<p></p>

But what about implementing polymorphism in the DB? Its quite common to handle programming requirements like:

1. a `user` on Github has `permissions` for `repos, teams`
2. an `chat_message` can belong to a `company, user, staff`
3. a `upvote/downvote` can belong to a `post` or a `comment`

<p></p>


We could solve the above problems with polymorphism, and some quick Googling would return a few ways to implement polymorphic relationships in your DB.  E.g.

1. [Single Table Inheritance](http://www.martinfowler.com/eaaCatalog/singleTableInheritance.html)
2. [Class Table Inheritance](http://www.martinfowler.com/eaaCatalog/classTableInheritance.html)
3. [Concrete Table Inheritance](http://www.martinfowler.com/eaaCatalog/concreteTableInheritance.html).

<p></p>

However, we are not going to delve into the specifics of these  implementations in this post. Rather, I want to cover when and why should you use/avoid using polymorphic relationships in your database.
<br>
<br>

## Tradeoffs when you implement Polymorphism in your DB

As with anything in programming, there are always tradeoffs involved when picking a specific implementation over another.

### Pros

  1. Less duplication of repeated columns/data
  2. Your chosen ORM may have a specific implementation for polymorphic tables, enables code standardization
  3. May make code more adaptable

### Cons

  1. May make code more adaptable, and thus more unpredictable
  2. May violate foreign key integrity, most databases are not able to enforce polymorphic foreign key relationships
  3. More complexity in indexing your tables
  4. Unable to use database diagramming/design tools to infer polymorphic relationships
  5. Less data integrity, e.g. when implementing STI, a row may contain data from other models.
  6. Reduced performance when querying, especially when joining tables.

<br>
<br>


## When should you implement DB Polymorphism

Besides the tradeoffs above, there is one general rule I like to apply when considering whether I should implement DB polymorphism, that being:

*Do you need to aggregrate data?*

Take the `upvotes/downvotes` on `posts` and `comments` for example. It seems intuitive to create one `votes` table and link it to both the `posts` and `comments` tables.

Consider however, do you ever need to aggregrate both types of `votes` in one query? Will you ever be making queries for things like: User 1 has made 80 upvotes and 20 downvotes for posts and comments. I would say that's pretty unlikely. The votes seem to share the exact same attributes, but you never really need to consider them as one **type* of object, you always consider them separately.

It may be better to seperate the tables on the DB level, and implement code sharing for common functionality on the application level instead. As a bonus, your querying performance would improve since there are less rows per table to process, especially when joining the tables.

On the other hand, lets consider a item in a online course. The item could be a `video-only-lesson`, a `text-only-lesson`, a `quiz`, or an `assessment`

Each type of content has different attributes, you might think that you should not implement polymorphism at all! But they all need to implement a specific attribute, a `item_order` that can be adjusted by the course management admin.

In this situation, you would need to implement some level of DB polymorphism its make querying for the next and previous `content` much easier, especially if you have some kind of course hierachy, e.g. course has many modules, modules has many items. It is going to be far easier to just make the SQL query `ORDER BY modules.order, items.item_order` instead of on the application level.

Hope this helps in your decision making!
