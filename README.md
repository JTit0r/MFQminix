# MFQminix
Objective: modify the current minix scheduler to operate as a Multilevel Feedback Queue (MFQ).

To do so, the following modifications should be made to the source code:

• Adding an unsigned quantum in schedproc.h file. This file
is found under usr/src/servers/sched/

• Added 3 queues, that are used for the different levels in the
MFQ. These queues are added in /usr/src/include/minix/config.h.
Where the original number of queues were changed from 16 to 19.
Now the the number of NR queues are 19 whereas the number of user
queues are 16.

CHANGES MADE IN THE MAIN SCHEDULER:
There were several modifications made in the schedule.c in
the directory /usr/src/servers/schedule.c.

• do_noquantum( ): For processes between queue 16 and 18,
increase their priority by 1, as soon as their quantum expires. For
processes in queue 18, set the new priority as 16 .For all other
processes, use default policy.

• do_start_scheduling( ): In this function we mainly initialise the
priority, max_priority, quantum, and time_slice. Changing the value’s
when required.

• do_nice( ): If we cannot assign new priorities to the jobs in
queues 16 -18, we assign previous priority value’s. In case of an
error we roll back.

• balance_queues( ): increase the priorities of all the jobs by
one.

Official MINIX sources - Automatically replicated from gerrit.minix3.org
