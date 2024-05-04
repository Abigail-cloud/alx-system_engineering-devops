0x05-processes_and_signals
##Processes and Signals##
##Process definition: Address space; threads (program(s)) running inside that space); required system resources for those threads.

Process contents: program code, data, variables in system memory, open files (file descriptors), and an environment space filled with environment variables, and its own program counter to keep track of which statement it is executing in each of its threads. (Linux actually shares one copy of code used by more than one process – like windows DLL.)

PROCESS STAT CODES

       Here are the different values that the s, stat and state output specifiers (header "STAT" or "S") will display to describe the state of a process: from http://askubuntu.com/questions/360252/what-do-the-stat-column-values-in-ps-mean

·         D    uninterruptible sleep (usually IO)

·         R    running or runnable (on run queue)

·         S    interruptible sleep (waiting for an event to complete)

·         T    stopped, either by a job control signal or because it is being traced.

·         W    paging (not valid since the 2.6.xx kernel)

·         X    dead (should never be seen)

·         Z    defunct ("zombie") process, terminated but not reaped by its parent.

·         <    high-priority (not nice to other users)

·         N    low-priority (nice to other users)

·         L    has pages locked into memory (for real-time and custom IO)

·         s    is a session leader

·         l    is multi-threaded (using CLONE_THREAD, like NPTL pthreads do)

·         +    is in the foreground process group.
