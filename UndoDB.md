# Reverse debugging WiredTiger with UndoDB

---

# Background

- The most time-consuming bugs are those that tend to not reproduce often.
- Even when bugs do reproduce somewhat frequently, we can have other issues:
  - If a bug affects different keys, pages, etc, it can be hard to know where to place breakpoints.
  - Printing lots of diagnostic information can be good but can affect reproducability.

---

# What is UndoDB?

- UndoDB is a reverse debugger for Linux C/C++ applications.
- It provides:
  - Deterministic reproducers for bugs.
  - A GDB-like interface for stepping back and forth through these reproducers.

---

# How do I use it?

UndoDB comes in two parts:

live-record: A program that records the runtime of an application.

```
$ live-record ./foo arg1 arg2 arg3
```

udb: A program to replay and debug the replay captured with live-record.

```
$ udb foo.record
```

---

# Demo

---

# Format integration

We often use format.sh to reproduce format bugs.

The -r flag allows each format process to be run under live-record.

```
$ ./format.sh -r live-record
```

---

# Resmoke integration

The resmoke.py script used for MongoDB testing also supports recording mongod instances with live-record.

```
$ ./buildscripts/resmoke.py run --recordWith live-record [your other resmoke args]
```

---

# How do I get it?

There are two ways of getting your hands on a copy of UndoDB:
- Spawn a virtual workstation. The base image has live-record and udb installed by default.
- Install manually on your own machine.
  - Instructions at https://wiki.corp.mongodb.com/display/KERNEL/UndoDB+Usage.

---

# How is this different from Mozilla's rr?

UndoDB imposes less overhead than rr meaning bugs are more likely to reproduce.

rr uses hardware performance counters for its recordings which can be an issue in some virtualised environments.

UndoDB is proprietary and closed-source software.

---

# Is this a silver bullet?

Probably not.

If a bug is already difficult to reproduce, recording it will be harder.
