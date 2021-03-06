Simplerun
=========

[![Build Status](https://travis-ci.org/netspyer/simplerun.png?branch=dev)](https://travis-ci.org/netspyer/simplerun)
[![Downloads](https://pypip.in/d/simplerun/badge.png)](https://crate.io/package/simplerun)

Make shell script running in a Pythonic way.

Installation
============

    pip install simplerun

or

    python setup.py install

Usage
=====

Basic usage:
------------

    >> import simplerun
    >> r = simplerun.run('ls -l')
    >> r
    <[0] `ls -l`>

*0* refer to the exit code here.


    >> print(r.std_out)
    total 8
    -rw-r--r-- 1 yj staff 664 11 19 21:46 README.md
    -rw-r--r-- 1 yj staff 829 11 18 22:39 setup.py
    drwxr-xr-x 6 yj staff 204 11 19 21:46 simplerun


Iterable as input:
------------------

    >> with open('simplerun.py') as f:
    >>    r = simplerun.run('grep def', f)
    >>    print(r.std_out)


Result as input:
----------------

    >> r_data = simplerun.run('ps aux')
    >> r = simplerun.run('grep Chrome', r_data)

Debugger:
---------

    >> r = run('ps aux | stranger | grep keyword')
    >> r
    <[-1] `stranger`>
    >> r.exc
    OSError(2, 'No such file or directory')

Found the `stanger` is the evil

    >> r.history
    [<[0] `ps aux`>]

Show history, and run it again with good input:

    >> r.rest
    [['grep', 'keyword']]
    >>
    >> r2 = run(r.rest, '''This is the good line that contains the keyword
                           but not this line, sorry.''')
    >> r2
    <[0] `grep keyword`>
    >> print(r2.std_out)
    'This is a good line that contains the keyword\n'


Concurrent & Parallel:
----------------------

    >> batches = ['ps', 'top -n 10', 'uptime']
    >> r = prun(batches)
    >> r2 = crun(batches)
