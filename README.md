﻿Redis on Windows prototype
===
## What's new in this release

- Important note: we have changed our branch names to make it clear which is the main branch, and the Redis version the branch is based on.
- Now have x64 version as well as 32 bit versions.
- For the 64 bit version, there is a limit of 2^32 objects in a structure, and a max length of 2^32 for any object
- Version number now 2.4.11-pre1 to indicate prerelease and to enable changing
- The code to write data over TCP has been improved. Replication under stress is now more reliable.
- Fixes for a few reported issues.

From previous releases
- Based on Redis 2.4.11
- Removed dependency on the pthreads library
- Improved the snapshotting (save on disk) algorithm. Implemented Copy-On-Write at the application level so snapshotting behavior is similar to the Linux version.

===
Special thanks to Dušan Majkic (https://github.com/dmajkic, https://github.com/dmajkic/redis/) for his project on GitHub that gave us the opportunity to quickly learn some on the intricacies of Redis code. His project also helped us to build our prototype quickly.

## Repo branches
<<<<<<< HEAD
- 2.4: save in foreground
- bksave: background save where we write the data to buffers first, then save to disk on a background thread. It is much faster than saving directly to disk, but it uses more memory. 
- bksavecow: Copy On Write at the application level. Has lower latency than bksave.
=======
- 2.4: (formerly bksavecow) Copy On Write at the application level, now the default branch. Has lower latency than bksave.
- 2.4_fgsave: (formerly 2.4) Saving is done in foreground
- 2.4_bufsave: (formerly bksave) Background save where we write the data to buffers first, then save to disk on a background thread. It is much faster than saving directly to disk, but it uses more memory. 
>>>>>>> 33bde01... update README

## How to build Redis using Visual Studio

You can use the free Express Edition available at http://www.microsoft.com/visualstudio/en-us/products/2010-editions/visual-cpp-express.

<<<<<<< HEAD
- The new application-level Copy On Write code is on a separate branch so before compiling you need to switch to the new branch:
<pre><code>git checkout bksavecow</code></pre>
    
=======
Now *2.4* is the default branch.
>>>>>>> 33bde01... update README

- Open the solution file msvs\redisserver.sln in Visual Studio 10, select platform (win32 or x64) and build.

    This should create the following executables in the msvs\$(Configuration) folder:

    - redis-server.exe
    - redis-benchmark.exe
    - redis-cli.exe
    - redis-check-dump.exe
    - redis-check-aof.exe


<<<<<<< HEAD
### Release Notes

This is a pre-release version of the software and is not yet fully tested, because of that we are keeping the Fork/COW code on a separate branch until testing is completed.  
This is intended to be a 32bit release only. No work has been done in order to produce a 64bit version of Redis on Windows.
To run the test suite requires some manual work:
=======
## RedisWatcher
A Windows Service that can be used to start and monitor one or more Redis instances, the service 
monitors the processes and restart them if they stop. 

You can find the project to build the service under the msvs\RedisWatcher directory. In the readme on the same location
you will find the instructions on how to build and use the service.

## RedisWAInst
An installer to deploy Redis to Windows Azure. Source and binaries are in msvs/RedisWAInst. See the README file there for details.

## Release Notes

This is a pre-release version of the software. It has been undergone more testing than previous releases.

To run the Redis test suite requires some manual work:
>>>>>>> 33bde01... update README

- The tests assume that the binaries are in the src folder, so you need to copy the binaries from the msvs folder to src. 
- The tests make use of TCL. This must be installed separately.
- To run the tests you need to have a Unix command line shell installed. To run the test: `tclsh8.5.exe tests/test_helper.tcl`. If a Unix command shell is not installed you may see the following error: “couldn't execute "cat": no such file or directory”.

### Plan for the next release

- Improve test coverage
- Fix some performance issues on the Copy On Write code


 