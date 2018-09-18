---
title: The nobody user in Unix
layout: default
---
# The `nobody` user in Unix

I stumbled across a `Dockerfile` whith the following line

```Dockerfile
USER nobody
```

It was actually preventing the thing from working but I wanted to understand why was it choosen to use this user and what is the purpose of having the `nobody` user.

The first good thing seems to be that it’s always present and it’s not even a proper user, it’s a “pseudo-user”. It’s the conventional name for the user with the least privileges.

So it would seem that the user `nobody` is used to limit damages when launching a deamon or any software that could behave erratically. Not really the case in a dockerized application which can’t by default get out from its sandbox.

Still, according to [this article, processes in containers should not run as root](https://medium.com/@mccode/processes-in-containers-should-not-run-as-root-2feae3f0df3b). The reason being mainly security and limiting the possible attack surface.

But then the article goes on explaining how to create your default user. Instead using the `nobody` user seems a much more attractive option since it’s already there.