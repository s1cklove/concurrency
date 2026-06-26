# Concurrency-related stuff!

Something to look at if you want to learn more about concurrency. Not sure if it has any target audience, but at least a place for me to "pin" or TODO stuff.

## Great examples

### [tokio](https://github.com/tokio-rs/tokio) (Rust)
A runtime to steal ideas from. Very complicated; good luck to grep whatever you want. Haven't looked much through it, still worth mentioning.

### [Go Runtime](https://github.com/golang/go/tree/master/src/runtime) (Go) 
More speaking-for-itself one. Things are mostly obvious or explained it long self-referring comments (or even documents: see [HACKING.md](https://github.com/golang/go/blob/master/src/runtime/HACKING.md)). We're mostly interested in [proc.go](https://github.com/golang/go/blob/master/src/runtime/proc.go). The amount of great ideas in there is insane!

### [ClickHouse's Silk](https://github.com/ClickHouse/silk) (C++) 
Announced [recently](https://clickhouse.com/blog/silk), so I don't have much to tell about it -- even though it was _everywhere_ at the moment of announcement. Likely a TODO, as it's the first example of a well-discussed C++ runtime I've seen in a while. (YTsaurus devs stay away)

## (Small) Articles

### [Are Lock-Free Concurrent Algorithms Practically Wait-Free?](https://arxiv.org/pdf/1311.3200)
Short answer: yes. Making some assumptions on your scheduler algorithm, you can assume that wait-free algorithms usually don't make sense in practice and only stand out as theoretical results.

### [Iteration Inside and Out](https://journal.stuffwithstuff.com/2013/01/13/iteration-inside-and-out/)
A **really** fun one I'd recommend to anyone who just starts their CS path (even sent it to my 1st grade students during code review at some point!), or the ones who, for some reason, is not familiar with different kinds of iteration.

### [Semaphores are Surprisingly Versatile](https://preshing.com/20150316/semaphores-are-surprisingly-versatile/)
Just kinda fun. Expects you to be familiar with basic concurrency, but doesn't provvide any complicated stuff. Introduction speaks for itself.

## Literature
To be honest, I haven't _completely_ read any of these, but usually looked for some specific topic. Not taking responsibility if you want to read-through all of it.

### [Algorithms for Scalable Synchronization on Shared Memory Multiprocessors](https://web.mit.edu/6.173/www/currentsemester/readings/R06-scalable-synchronization-1991.pdf)
Relatively small set of chrestomatic algorithms that you might look through if you have time (...or you need to implement one).

### [A Science of Concurrent Programs](https://lamport.azurewebsites.net/tla/science.pdf)
To be fair about it, you should first read its contents and decide: does it _excite_ or _frighten_ you? If excite, _(and you have really lot of free time)_ -- you're probably a mathematician. Take it. I'm absolutely amazed for what does it present. For all others, you must not be alone.

### [The C10K problem](https://kegel.com/c10k.html)
A long long TODO. Mostly putting it here to look through someday.

## Other

### [The Deadlock Empire](https://deadlockempire.github.io/#menu)
A DEADLOCK GAME. Yeaaaaaaaaah.

### [Awesome Concurrency](https://gitlab.com/Lipovsky/awesome-concurrency)
A grandmaster repo that contains the most of this file contents and HUNDREDS or other ones. I'm not even kidding, this is insane.
