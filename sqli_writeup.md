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
![error](001.png)