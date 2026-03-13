
# Containers and the Illusion of Isolated Worlds
*Or how often we assume our environment is the whole universe*

## Containers  - A small universe in a box

Modern software rarely runs directly on a server anymore.

Instead, it often runs inside something called a **container**.

At a very high level, a container is a packaged environment that includes everything an application needs in order to run:

- the application itself
- its libraries
- its dependencies
- its configuration


All neatly bundled together.

This makes software easier to move.

The same container can run on a developer’s laptop, a testing server, or a production cluster somewhere in the cloud — ideally behaving the same way everywhere.

Which, in the unpredictable world of software, already feels like a minor miracle.

Containers are usually described as **isolated environments**.
The description is useful. 
It is also slightly misleading.

Like most descriptions in technology, this is **true enough to be useful and incomplete enough to be dangerous.**

Small, self-contained worlds where an application can run without interfering with the rest of the system.

Or at least, that’s the marketing version.

In reality, containers are not separate operating systems.  
They are not miniature virtual machines.

They are something subtler.

A container is essentially **a group of processes that the Linux kernel carefully hides from the rest of the system**.

From the inside, it looks like a complete environment.

From the outside, it is simply another set of processes running on the same machine.

Which raises an interesting question.

If a container believes it is its own small world…  
what does the beginning of that world look like?

To answer that, we have to look at how a Linux system starts.

Because every universe, even a small one, begins with **process number one**.



## Process number 1 - The first spark 

*In Greek mythology, Prometheus brought fire to humans.*  
*In a Linux system, process 1 plays a similar role.*  
*It is the first spark from which everything else emerges.*

Every Linux system begins with a single process.

Process number one.

The ancestor of everything else.

When the operating system finishes its early boot sequence, it launches the first userspace program. Traditionally this program is called **`init`**, although modern systems often use more complex replacements.

But the number remains the same.

Process **1**.

From that moment forward, every other process in the system is either created by it or descended from something it created. The entire forest of running programs traces its lineage back to this single root.

If process 1 dies, the system enters a kind of existential crisis.

Because if the ancestor disappears, the operating system no longer knows how to manage the rest of the family tree.

So the system collapses.

A small universe, suddenly without gravity.


### The strange thing about containers

Inside a container, something peculiar happens.

A new process becomes **process number one**.

From inside the container, it appears perfectly normal.  
It has PID 1.  
It launches other processes.  
It manages its own tiny ecosystem.

From its perspective, it is the beginning of everything.

Of course, it isn’t.

It is just a carefully maintained illusion.

The container is not a new operating system.  
It is not even a virtual machine.

It is simply a set of processes that the Linux kernel has been instructed to **hide from the rest of the system**.

This hiding is accomplished through several mechanisms.

Primarily:

- **Namespaces**
- **Control groups (cgroups)**

Namespaces create separate views of the system:

- a different process tree (PID namespace)

- different network interfaces

- different mount points

- different hostnames


Inside this environment, processes see only their own world.

A container with ten processes believes the entire universe contains exactly ten processes.

It is, in a technical sense, a very confident misunderstanding.


## The kernel:  hidden layer of reality 

*In Greek mythology, souls had to cross the river Styx to reach the deeper layers of existence.*

Despite this isolation, containers share something fundamental.

They all run on the **same kernel**.

That means the container’s universe exists only because the host system allows it to exist.

The container may believe it has its own process tree.  
But the host can see everything.

From the host’s perspective, containers are simply **groups of ordinary processes**.

This asymmetry is important.

Inside the container:

> "We are the world."

Outside the container:

> "You are just a few processes with extra boundaries."

This is why containers are lightweight.

A virtual machine needs its own kernel.

A container borrows one.

Which is efficient.

And also slightly dangerous.


## Security Implications — Every World Needs a Guardian

*In Greek mythology, the entrance to the underworld was guarded by Cerberus — a creature whose entire purpose was to prevent things from escaping.*
*Security systems serve a similar purpose.*

Because containers share the host kernel, **the isolation is not absolute**.

If a vulnerability exists in the kernel, a process inside a container may be able to escape its carefully constructed universe.

This is known as a **container breakout**.

From a security perspective, containers are therefore not tiny virtual machines.

They are better understood as **well-behaved prisoners sharing the same building**.

Most of the time, the walls hold.

But if the structure of the building itself is compromised, the boundaries become negotiable.

This is why container security focuses heavily on **reducing trust**.

Not eliminating risk.

Reducing it.

## Best Practices for Container Security - Keeping Small Universes Stable

If containers create small universes, security engineers must assume those universes might eventually misbehave.

Software, much like human societies, has a remarkable talent for finding creative ways to break its own rules.

Fortunately, a few practical precautions can keep these small worlds reasonably well behaved.

### Run containers as non-root

Many containers still run as **root** by default.

This is convenient.

It is also unnecessary in most cases.

Running as root inside a container gives a process far more authority than it actually needs. If that process becomes compromised, the attacker inherits the same privileges.

Running containers as a **non-root user** limits the damage.

Even in small universes, absolute power rarely ends well.


### Drop unnecessary Linux capabilities

In Linux, the powers traditionally associated with the root user are divided into smaller permissions called **capabilities**.

Most applications require only a handful of them.

Yet many containers run with far more privileges than necessary.

Removing unused capabilities reduces the number of ways a compromised process can interact with the system.

Less power usually means fewer interesting ways to misuse it.


### Avoid privileged containers

A **privileged container** has broad access to the host system.

At that point, the idea of isolation becomes… somewhat ceremonial.

Privileged containers can be useful for specialized tasks, but they should be treated with extreme caution.

Granting that level of access is less like opening a window and more like **removing the wall entirely**.


### Use seccomp and security profiles

Tools like **seccomp**, **AppArmor**, and **SELinux** restrict which system calls a container is allowed to make.

Think of them as rules that quietly define what actions are even possible inside that world.

If a process becomes compromised, these restrictions can prevent it from doing anything particularly adventurous.

Even mischievous programs have a harder time causing trouble when the list of allowed actions is very short.


### Keep the host system patched

Because containers share the same kernel, the **host operating system becomes the real security boundary**.

If the host kernel is vulnerable, every container inherits that weakness.

Which means that the most sophisticated container security strategy still depends on one very traditional practice:

keeping systems updated.

It is not glamorous.

It rarely appears in conference talks.

But it remains one of the most reliable defenses ever invented.

Still undefeated.


## Systems Within Systems

*Even ancient cosmologies described reality as layers of worlds stacked upon each other.*

Containers are a useful metaphor.

Inside each container is a world that appears complete.

Processes interact with each other.  
They communicate.  
They perform useful work.

From their perspective, the system they see is the entire system that exists.

But above them sits a larger environment that defines the rules of their universe.

The host.

And above that host, there might be another host.

And another.

A container inside a VM inside a cloud hypervisor inside a data center somewhere in the physical world.

Systems within systems.

Realities nested inside larger realities.


## A small thought experiment

Imagine a dartboard.

On that dartboard is a single dot.

That dot represents **our reality**.

The one we assume is the base layer.

The original system.

Now close your eyes and throw a dart.

If the dart lands exactly on our layer of reality, congratulations — we are the base system.

Statistically speaking, that would be an impressive throw.

But what really are the chances that you hit that exact dot?

If intelligent beings can create simulated environments...

And those environments can create further environments...

Then each level might believe it is the beginning of everything.

A character in a video game may believe it has free will.

Inside that game, it might even create something else.

A simulation within a simulation.

A container inside another container.

And each layer would assume its perspective is the center of the universe.

The probability that we are standing on the **very first layer** begins to look... statistically optimistic.


## Systems and humans

Humans do something remarkably similar.

We experience reality from a single point of view and instinctively assume that perspective is central.

It feels like the universe revolves around our observations.

Containers behave the same way.

Inside the container, the system looks complete.

Self-contained.

Meaningful.

Until something outside the container decides otherwise.


## A quiet lesson from systems engineering

Good engineers eventually learn a simple rule:

> The system you see is rarely the entire system.

There is almost always another layer.

Another dependency.

Another boundary.

Another assumption quietly holding everything together.

Containers remind us of that.

They are not fully isolated worlds.

They are **carefully constructed perspectives**.

Useful.

Efficient.

And slightly humbling.

Because from the outside, every universe eventually looks like just another process.

