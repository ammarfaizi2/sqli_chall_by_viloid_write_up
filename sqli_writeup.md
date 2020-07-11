# (1) Analyzing Target
Given the following URL
```
https://sphere.social/hello/search/hashtags/1
```
We know that "1" is the parameter which may involve query to database.

After several times trying to find the injection pattern, I found an error at this endpoint.

```
https://sphere.social/hello/search/hashtags/"
```

and this is what I got. The developer seems did not turn off the debug flag, so it is easier to see what query is being executed. The error looks like this:
![error](./001.png)

If you are experienced in SQL injection, the above situation must be a low-hanging fruit which is easily to be injected. So I tried the following payload:
```
" and 0 union select 1,2,3,4,5,6 -- -+
```

The endpoint become:
```
https://sphere.social/hello/search/hashtags/%22+and+0+union+select+1%2C2%2C3%2C4%2C5%2C6+--+-%2B
```

It is then not that easy, Cloudflare's firewall blocked my attempt and showed me this page:
![error](./002.png)