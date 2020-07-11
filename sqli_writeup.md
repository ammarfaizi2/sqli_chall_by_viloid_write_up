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
![xxx](./001.png)

If you are experienced in SQL injection, the above situation must be a low-hanging fruit which is easily to be injected. So I tried the following payload:
```
" and 0 union select 1,2,3,4,5,6 -- -+
```

The endpoint become:
```
https://sphere.social/hello/search/hashtags/%22+and+0+union+select+1%2C2%2C3%2C4%2C5%2C6+--+-%2B
```

It was then not that easy, Cloudflare's firewall blocked my attempt and showed me this page:


![xxx](./002.png)


I was trying to bypass the Cloudflare's firewall with encoding, comment technique and many more, but no success.


But then I realized that Cloudflare is just a DNS proxy.


So basically if we can find the real address of the server behind the Cloudflare, we can easily bypass this extreme firewall.

# (2) Finding Server's IP Address Behind The Cloudflare

I tried to use `whois` to find the domain information, but no success.


The only necessary I got is just the domain was bought from godaddy.

![error](./003.png)

Futhermore, I tried to find the DNS history by using the server from https://viewdns.info/ and this is what I got:

![error](./004.png)


So my conclusion based on the DNS history above was:
1. `50.63.202.45` was the default page when the domain was bought.
2. `50.28.107.96` is the IP address after domain was configured.
3. Then the domain nameserver was moved to the Cloudflare.

Therefore, it is possible that `50.28.107.96` be the real server address behind the Cloudflare.

I tried to check the HTTP port on this address and it redirected me to `https://sphere.social/`. This is enough evidence that the server behind the Cloudflare is `50.28.107.96`.

