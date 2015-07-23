# SQL_to_ActiveRecord

By:

* [David Meza](https://github.com/david-meza/)
* [Hi](https://github.com/)

Assignment part of [Viking Code School](http://www.vikingcodeschool.com/) curriculum

## Assignment: SQL to Active Record Translations

### Translate from SQL to Active Record

    User.all
    SELECT *
    FROM
      users;

    User.where("id = 1")
    User.find(1)
    SELECT *
    FROM
      users
    WHERE
      user.id = 1;

    Post.all.order("created_at, DESC").limit(1)
    Post.last
    SELECT *
    FROM
      posts
    ORDER BY
      created_at DESC
    LIMIT 1;

    User.joins("posts").where("posts.created_at >= '2014-08-31 00:00:00'")

    SELECT *
    FROM
      users
    JOIN
      posts
    ON
      posts.author_id = users.id
    WHERE
      posts.created_at >= '2014-08-31 00:00:00';

    User.group(:favorite_color).count
    User.select("count(*) AS user_count_by_color").group("favorite_color").having("count(*)>1")
    SELECT
      count(*)
    FROM
      users
    GROUP BY
      favorite_color
    HAVING
      count(*) > 1;


    * The most recently updated user
    User.order("updated_at").last

    * The oldest user (by age)
    User.select("user").having("MAX(age)")
    User.order("age DESC").limit(1)

    * all the users
    User.all

    * all posts sorted in descending order by date created
    Post.order("created_at, DESC")

###Translate from Active Record to SQL

    Post.all
    SELECT * FROM posts

    Post.first
    SELECT * FROM posts LIMIT 1

    Post.last
    SELECT * FROM posts ORDER BY id DESC LIMIT 1

    Post.where(:id => 4)
    SELECT * FROM posts WHERE id = 4

    Post.find(4)
    SELECT * FROM posts WHERE id = 4 LIMIT 1

    User.count
    SELECT count(*) FROM users

    Post.select(:name).where(:created_at > 3.days.ago).order(:created_at)
    SELECT name FROM posts WHERE created_at > 2015-07-20 ORDER BY created_at

    Post.select("COUNT(*)").group(:category_id)
    SELECT count(*) FROM posts GROUP BY category_id

    All posts created before 2014
    SELECT * FROM posts WHERE created_at < 2014-01-01 00:00:00

    A list of all (unique) first names for authors who have written at least 3 posts
    SELECT DISCTINCT first_name FROM Authors JOIN Posts ON Authors.id = Posts.author_id GROUP BY author.id HAVING count(posts.id)>=3

    The posts with titles that start with the word "The"
    SELECT * FROM posts WHERE title LIKE "The%"

    Posts with IDs of 3,5,7, and 9
    SELECT * FROM posts WHERE id IN (3,5,7,9)

### Custom Queries


* Select authors who wrote more than 3 posts containing title "Ruby" (any word)

    SELECT authors.name, COUNT(posts.title)
    FROM posts JOIN authors ON (posts.author_id = authors.id)
    WHERE posts.title LIKE ('Ruby%')
    GROUP BY author_id
    HAVING COUNT(posts.title) > 3

    Post.select("authors.name, COUNT(*)").join(posts).where(:title LIKE 'Ruby%').group(:author_id).having("count(title) > 3")



* Select all users who have not written any articles

    SELECT authors.name
    FROM authors LEFT JOIN posts ON authors.id = posts.author_id
    WHERE posts.id IS NULL


    Author.select(:name).join("LEFT JOIN posts ON authors.id = posts.author_id").where(:posts.id => nil)


* Count all the users who registered at a particular date

    SELECT *
    FROM users
    WHERE created_at = 2015-07-23

    User.where("created_at = 2015-07-23")









