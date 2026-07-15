[[Quant/index|Quant/index]]

a random youtuber: you cannot observe the market without disturbing it
after internship my understanding: you can find an edge without disturbing it, but you cannot exploit an edge without eroding it

before joining an hft:
- yeah a hft runs in nanoseconds
- they just do market making, hold their positions
- they use a lot of DSA CP complex algos to optimize their c/cpp code
- spsc queue :dont make me laugh: they are simple
- event driven is better than polling
- except for predetermined simple task (can be ran on fpga), code has to run on cpu cores
- writing the fastest matters the most for hfts
- hfts dont care about resource constraint, less bandwidth, memory, compute -> buy more
- who cares about endianess


after internship:
- i benchmarked the system and mostly it does takes nanos to few micros, benchmarking and performance measurement is as important as testing of workings and finding edge cases for a piece of code
- a lot is going in hfts, a lot !
- most of optimizations that DSA CP teaches us are pretty obvious, firms dont usually implement them from scratch, a lot of open source code has highly optimized DSA, its like general knowledge. the actual optimizations are usually things that are never taught in colleges or courses or by youtubers, you can only understand them if you have worked in low latency code and have deep understanding of concepts from OS, Networking, Compilers, Memory, Hardware etc.. (what i like to call, actual essence of what a "code" is)
- one of the most used data structure in hft firms is not some fancy DP Graphs algo, its our simple, devilish queue, for ones still confused watch some cppconf youtube videos and use compiler explorer to optimize them
- depending on use case and data, was always a bit susceptible to fully agree but never saw such strong example for polling better than event driven
- hfts use custom hardware which may or may not have even custom chips for very specific complex operations (not like fpga, these chips have rather "wired operations")
- its kinda true that fastest code wins and that what every hft firm is after but monitoring and alerting of the systems is moooossst important thing, you dont wanna loose millions in micros rights ?
- they care, search about co-location servers and their maintenance cost
- no comments

some good resources:
- cpp conf videos (best)
- OS - Dinosaur book
- Networking - computer networking : a top-down approach
- Compilers - start from [here](https://aosabook.org/en/v1/llvm.html), then llvm docs, use compiler explorer to experiment
- what an hft do ? - akuna capital option 101 course (free pretty straight forward good for foundations, btw india has largest option market)

