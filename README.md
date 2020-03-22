# A trivial web page

The webpage, index.html, has "Hello".

I used this to test accessing a this webpage which currently
is hardcoded to my AT&T public address and the "runs" below
show that accessing it you need to sleep about 20 to 25secs
between `curl ...` commands, any less and the "time" output
is much greater than the 17ms we see.

# Start web server
```
(base) wink@wink-desktop:~
$ cd trivial-web-page
(base) wink@wink-desktop:~/trivial-http-page (master)
$ python -m http.server 8080
Serving HTTP on 0.0.0.0 port 8080 (http://0.0.0.0:8080/) ...
```

In another terminal run curl
```
(base) wink@wink-desktop:~/trivial-http-page (master)
$ curl localhost:8080
Hello
```

And you see http server output the request:
```
(base) wink@wink-desktop:~/trivial-http-page (master)
$ python -m http.server
Serving HTTP on 0.0.0.0 port 8080 (http://0.0.0.0:8080/) ...
127.0.0.1 - - [21/Mar/2020 20:18:11] "GET / HTTP/1.1" 200 -
```

# Some runs

These runs are after forwarding port 2000 to 192.168.1.100,
my linux desktop.  My home is at 23.119.164.150. So the curl
command in `req` is `time curl http://23.119.164.150:2000/index.html`.
And this shows that from some reason the first accesss is "always"
fast, < 20ms. And the subsequent ones are slow if the delay is < 20 or 25
seconds. I believe this is because some "throttling" in the AT&T
BGW210-700 router which causes it to drop TCP SYN packets if they come
to rapidly.

```
(base) wink@wink-desktop:~/trivial-http-page
$ ./req 25 4
0 curl
Hello

real	0m0.015s
user	0m0.004s
sys	0m0.004s
done curl
sleeping 25
1 curl
Hello

real	0m0.017s
user	0m0.006s
sys	0m0.003s
done curl
sleeping 25
2 curl
Hello

real	0m0.016s
user	0m0.008s
sys	0m0.001s
done curl
sleeping 25
3 curl
Hello

real	0m0.016s
user	0m0.008s
sys	0m0.001s
done curl
sleeping 25
4 curl
Hello

real	0m0.015s
user	0m0.008s
sys	0m0.001s
done curl
(base) wink@wink-desktop:~/trivial-http-page
$ ./req 5 3
1 curl
Hello

real	0m0.017s
user	0m0.009s
sys	0m0.000s
done curl
sleeping 5
2 curl
Hello

real	0m15.347s
user	0m0.004s
sys	0m0.006s
done curl
sleeping 5
3 curl
Hello

real	0m15.264s
user	0m0.005s
sys	0m0.004s
done curl

(base) wink@wink-desktop:~/trivial-http-page
$ ./req 0 3
1 curl
Hello

real	0m0.016s
user	0m0.006s
sys	0m0.003s
done curl
sleeping 0
2 curl
Hello

real	0m33.049s
user	0m0.006s
sys	0m0.000s
done curl
sleeping 0
3 curl
Hello

real	0m32.424s
user	0m0.005s
sys	0m0.005s
done curl
```
