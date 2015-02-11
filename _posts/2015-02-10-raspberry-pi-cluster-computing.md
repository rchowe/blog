---
title: "Cluster Computing on Raspberry Pis"
layout: post
---

One of my interests is cluster computing -- the ability to share computing effort between multiple computers. I am also
a fan of the Raspberry Pi credit card sized computer: although it may not be the most cost effective in terms of
computing power per dollar, the computers are small, cheap, discrete computers, which makes them useful for learning
about cluster computing.
<!--more-->

Note, however, that the compute power per watt, and thus the compute power per dollar, on the Raspberry Pi will not be
as good as on a desktop machine, so if you want to mine bitcoins or improve your folding@home score, a cluster of
Raspberry Pis isn't for you. However, if you're curious about this whole high-performance computing thing and want to
dip your feet in, by all means buy a few pis and see how it works. Similarly, the Pis are limited by their low memory,
use of SD cards as local storage, and 10/100 ethernet port running over USB, which makes them slow.

### The Theory

_This section is on the theory of splitting jobs up into tasks; if you already know things about parallel computing, or
are only interested in how to get a cluster manager running on your Pi, feel free to skip it._

Cluster computing is predicated on the concept of breaking jobs up into tasks. A good example of a job to break into
tasks is a na&iuml;ve search for [prime numbers](https://en.wikipedia.org/wiki/Prime_number): given some number \\(n\\),
find out if it is prime or not. The following Python script does this, though not efficiently: if its input is prime, it
will print the input, and if its input is composite, it will print nothing.

{% gist rchowe/5b9103779ac33dcdaade %}

If I want to find all of the primes between 2 and 1000, I could use the following BASH script, which will run through
each number in order and check if it is prime or not. Because a result is only printed if a number is prime, this should
get me a list of all prime numbers between 2 and 1000.

{% gist rchowe/3468aa306d56a8e6e46d%}

But checking things one at a time is very slow, especially with a na&iuml;ve algorithm like this. Running this on my
MacBook Pro for numbers from 2 to 1000 takes about 21 seconds. However, there is nothing to stop us from checking all of
the prime numbers at once, using an inherent parallelism feature of BASH (the <code>&amp;</code> backgrounding
operator).

{% gist rchowe/677a1f706183518c080e %}

In this case, checking numbers from 2 to 1000 takes about 4.7 seconds. My MacBook has four cores, which account for the
roughly 4 times speed-up (the number is a little higher because of hyper-threading, i believe). However, the numbers are
printed out in a semi-random order. To get them in the same, sorted order that we got from running them sequentially, we
sort the numbers, a task that requires that all of the other tasks finish before it runs.

This simple example was running on my four-core MacBook Pro. Obviously, if we wanted to split the jobs up across
multiple computers in a cluster, we would have to write code to copy the script across computers, run the jobs, then
collect the outputs of the job, and perform some post-processing (like sorting), if necessary. Additionally, we need to
manage the resources we have to make sure that all 9998 of the jobs we submitted don't get dumped on one computer.
Conveniently, software called "resource managers" exists to handle most of this for us.

A resource manager knows the resources (number of cores, RAM) present on each node, and is able to divide up jobs across
nodes to best allocate those resources. On a more advanced (_i.e._ expensive) level, they are also able to ensure that
each user in a multiuser system gets a fair amount of computer time, that service level agreements are honored, and
other things useful to managers of large clusters. The most common open source resource manager is
[TORQUE](https://en.wikipedia.org/wiki/TORQUE), however others, both free and paid, exist. Some resource managers, such
as [Hadoop](https://en.wikipedia.org/wiki/Apache_Hadoop), also provide data storage and a whole host of other features,
however it is less lightweight than TORQUE and the equivalent schedulers. Although Hadoop [has been put on a Raspberry
Pi cluster before](http://www.raspberrypi.org/forums/viewtopic.php?t=37190), it requires modifications to
make it fit within the 512 MB memory footprint; the author of the linked article notes that every console command took
about 15 seconds to process. I have been able to effectively run interactive commands using both TORQUE and OpenLava on
my two-node Raspberry Pi cluster.

Of minor note: Hadoop more explicitly breaks tasks up into "maps" and "reduces": in this context, we map the
<code>check_prime.py</code> script to the array \\([2, 3, \ldots, 1000]\\) and reduce it by sorting the results. This
correspondence is convenient because of how I set up the <code>check_prime.py</code> script.

### Running it on Raspberry Pi

I have tried both TORQUE and [OpenLava](http://www.openlava.org) on my Pis, and I personally like OpenLava more because
I am more used to a LSF cluster, and because you can easily submit BASH commands by prefixing them with
<code>bsub</code>.

I decided to use an NFS share to share data between the Pis for jobs which needed it.
Many clusters are set up to share most non-device-specific directories on the filesystem, which works well for identical
hardware, since you can guarantee that, for instance, the version of Python that you're running is identical across all
of your computers. In this case, since I wanted to use one of the Pis as a network gateway and a different Pi for video
game emulation, I only share a single directory, <code>/data</code>. This means that anything outside of
<code>/data</code> is the wild west, and WILL NOT be shared between the computers. I'm relying on myself doing minimal
other things that affect the software that I want to run on the cluster: since I mostly want to run simple Python
programs, I need to ensure that I mainly need to ensure a usable Python 2.7 environment. In the future I may make
<code>/data</code> into a [chroot jail](https://en.wikipedia.org/wiki/Chroot) for a dedicated cluster user, and
cherry-pick the tools that I want to and include them in the jailed <code>/data/usr/bin</code>. However, for very simple
things, this setup works fine.

The OpenLava install, according to the [quickstart guide](http://www.openlava.org/documentation/quickstart.html), is
very easy. I replaced <code>make install</code> with <code>checkinstall</code> so that I generated a <code>.deb</code>
file, which interfaces with Debian's package manager and allows for easy removal.

{% gist rchowe/0003fd8d8a2969db9ac0 %}

Since the cluster was all Raspberry Pis, I copied the compiled source directory to each Pi and ran
<code>checkinstall</code>. I performed the other instructions, per the quick start guide on each pi, however I modified
the <code>lsf.cluster.openlava</code> to change the name of the cluster and list the names of each of the compute nodes.
I had some minor challenges getting the configuration files synced between each computer, however I sorted them out.
Below is an image of the master node on my cluster showing the cluster ID and the two available nodes I have added to
it:

![]({{site.baseurl}}/assets/cluster-id.png)

Now that the cluster is working, a good test for it is to find prime numbers between 2 and 100 (in this case we only
check 98 numbers because it creates a ton of job IDs)! Using the same Python script as before, this can easily be
parallelized using a familiar three lines of BASH:

{% gist rchowe/ee353dd8a3dbf6eb75d3 %}

The status of the jobs can be checked using the <code>bjobs</code> command. This submits a lot of jobs.
<code>bsub</code> does not block and wait for its command to finish, so you might have to wait until all of the jobs
are finished to get the output list. However, the sheer number of jobs is inconvenient for such short-running jobs:
LSF takes about half a second to process each job submission. For a large number of short jobs, this can be solved using
job arrays:

{% gist rchowe/0d2512b7dd5ae792678d %}

In this case, all of the jobs are submitted under the same ID, and parallelism is handled using the
<code>$LSB_JOBINDEX</code> environment variable. The jobs provided in this way are more lightweight, though the
additional effort required to make your parallelism dependent on the <code>$LSB_JOBINDEX</code> variable may not be
worth it.

### Caveats

There are a lot of ways to criticize the parallel code above. However, in general, programmer time is more expensive
than computer time and disk space. The above examples are simple and work for small- and medium-sized datasets, and can
be easily generated from existing sequential BASH scripts by putting <code>bsub</code> before the intense commands. So
before you go down a rabbit hole developing a massively parallel, fault-resistant, earthquake-tolerant "big data"
solution, think of how big your data really is, how much your time is worth, and the fact that I wrote these examples in
about two minutes.

Yes, There are additional tricky ways to make code like this faster, such as
[MPI](https://en.wikipedia.org/wiki/Message_Passing_Interface), however they are much more complicated. If you have a
tool which takes a long time, the additional half second per job dispatched individually with <code>bsub</code> probably
will not matter, and it might not be worth taking the time to write a wrapper script to use a job array or some other
convoluted solution to speed up the time taken in the cluster manager. If we really wanted to search the numbers from
2 to 1000, it would take about 8.5 minutes to submit all of the jobs (or 0.5 seconds if using a job array). However, if
the analysis takes more than about 10 minutes per sample, you've already saved yourself a lot of time by parallelizing
the job, and (depending on your cluster situation) your that 0.5 seconds per job may be taken out of the time the job
is sitting in the queue not doing anything anyway.

Also, realistically, one might (rightly) say that I am abusing the filesystem to store the prime numbers (cluster job
outputs) before sorting them, in a way that I did not have to do in the non-cluster examples. This is a problem
solved by using Map/Reduce or some pipeline framework on top of your cluster. However, for most tasks the output of the
command is more complex than counting prime numbers, and the output is useful diagnostically or forms a part of the
final product. This is also a consequence of confining my NFS share to a single directory and having the filesystem be
the main method of collecting the output. If you build a real HPC environment, you are very likely to
have tiered storage, such as fast temporary storage and slower permanent storage, which works well provided your goal
is not to process every tweet out of Twitter's firehose.
