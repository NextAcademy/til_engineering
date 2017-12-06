---
layout: post
title: "Bulk Updates with Join Tables in Postgresql"
author: "Ming Xiang Chan"
date: 2016-11-28 10:20:15 +0800
tags: [ruby, postgresql, rails]
preview: http://crosnt.com/croswp/uploads/2015/10/photodune-11496207-doctor-working-with-laptop-computer-in-medical-workspace-office-xs.jpg
---

Learnt a new trick for doing bulk updates in postgresql. I needed to update a many-to-many join table with attributes from the adjoining tables. Below is the raw SQL I used to handle it.

{% highlight sql %}
UPDATE user_courses
SET user_uuid = users.user_uuid,
    course_uuid = courses.course_uuid
FROM users, courses
WHERE user_courses.user_id = users.id
AND user_courses.course_id = courses.id
{% endhighlight %}

### Things to Note:

1. The Postgresql documentation does mention that the FROM statement would do a Cartesian Join of the tables, so this technique would only be applicable for inner joins.
2. For tables with a massive amount of rows. it may be more efficient to create a new table and insert the updated data into it. Please take a look at [this link](http://blog.codacy.com/2015/05/14/how-to-update-large-tables-in-postgresql/) for more information about this. TLDR, Postgresql actuall does 2 operations, INSERT and DELETE whenever you do an UPDATE operation, so its actually faster to create a new TABLE and only do INSERT operations on it.



