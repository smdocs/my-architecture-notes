# Notes on Distributed Systems
I’ve been thinking about the lessons distributed systems engineers learn on the job. A great deal of our instruction 
is through scars made by mistakes made in production traffic. These scars are useful reminders, sure, but it’d be better 
to have more engineers with the full count of their fingers.

New systems engineers will find the [Fallacies of Distributed Computing]() and the [CAP theorem]() as part of their 
self-education. But these are abstract pieces without the direct, actionable advice the inexperienced engineer needs to 
start moving. It’s surprising how little context new engineers are given when they start out.

Below is a list of some lessons I’ve learned as a distributed systems engineer that are worth being told to a new engineer.
Some are subtle, and some are surprising, but none are controversial. This list is for the new distributed systems engineer
to guide their thinking about the field they are taking on. It’s not comprehensive, but it’s a good beginning.

The worst characteristic of this list is that it focuses on technical problems with little discussion of social problems an 
engineer may run into. Since distributed systems require more machines and more capital, their engineers tend to work with 
more teams and larger organizations. The social stuff is usually the hardest part of any software developer’s job, and 
perhaps, especially so with distributed systems development. Our background, education, and experience bias us towards a 
technical solution even when a social solution would be more efficient, and more pleasing. Let’s try to correct for that. 
People are less finicky than computers, even if their interface is a little less standardized.

Alright, here we go.

#### 1. Distributed systems are different because they fail often.

When asked what separates distributed systems from other fields of software engineering, the new engineer often cites latency, 
believing that’s what makes distributed computation hard. But they’re wrong. What sets distributed systems engineering apart 
is the probability of failure and, worse, the probability of partial failure. If a well-formed mutex unlock fails with an 
error, we can assume the process is unstable and crash it. But the failure of a distributed mutex’s unlock must be built into
the lock protocol.
Systems engineers that haven’t worked in distributed computation will come up with ideas like “well, it’ll just send the 
write to both machines” or “it’ll just keep retrying the write until it succeeds”. These engineers haven’t completely 
accepted (though they usually intellectually recognize) that networked systems fail more than systems that exist on only a 
single machine and that failures tend to be partial instead of total. One of the writes may succeed while the other fails, and
so now how do we get a consistent view of the data? These partial failures are much harder to reason about.

Switches go down, garbage collection pauses make leaders “disappear”, socket writes seem to succeed but have actually failed
on the other machine, a slow disk drive on one machines causes a communication protocol in the whole cluster to crawl, and 
so on. Reading from local memory is simply more stable than reading across a few switches.


[](https://www.somethingsimilar.com/2013/01/14/notes-on-distributed-systems-for-young-bloods/)
