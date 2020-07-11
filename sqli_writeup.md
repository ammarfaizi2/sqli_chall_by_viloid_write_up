# Analyzing Target
Given the following URL
```
https://sphere.social/hello/search/hashtags/1
```
We know that "1" is the parameter which may involve query to database. After several times trying to find the injection pattern, I found error at this endpoint.

```
https://sphere.social/hello/search/hashtags/"
```

The error says:
```
Error: SQLSTATE[42000]: Syntax error or access violation: 1064 You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near '%"' at line 1
```

It also even show the executed query:
```
SQL Query: SELECT `Hashtag`.`id`, `Hashtag`.`item_id`, `Hashtag`.`item_table`, `Hashtag`.`hashtags`, `Hashtag`.`created`, `Hashtag`.`modified` FROM `sphere_network`.`hashtags` AS `Hashtag` WHERE hashtags LIKE "%"%" 
```