# Fork Description
I overridden the `int equalstr(const char *s, const char *s2)` function in diff.c,Because the original version of the comparison function will cause PE errors in the actual use process, and these errors are not necessarily caused by the problem of the submitter, it is likely to be caused by the negligence of the author, or even by the OS's different standards for newline characters.

Now the blank characters at the end are no longer compared, and for further convenience of the submitter, when two string pointers match to the blank characters at the same time, they will jump to the position of the next non blank character to start the comparison. The committers now don't have to think about handling some spaces or line breaks.

The above white space characters include the white space characters, '\r', '\n' and '\t'

Loco program runner core
========================

We use this python-c library to run program in a sandbox-like environment.
With it, we can accurately known the resource using of the program and 
limit its resource using including system-call interrupt.

Usage
-----

For run a program without tracing:

    runcfg = {
        'args':['./m'],
        'fd_in':fin.fileno(),
        'fd_out':ftemp.fileno(),
        'timelimit':1000, #in MS
        'memorylimit':20000, #in KB
    }
    
    rst = lorun.run(runcfg)

For check one output:

    ftemp = file('temp.out')
    fout = file(out_path)
    crst = lorun.check(fout.fileno(), ftemp.fileno())


trace
-----

You can set runcfg['trace'] to True to ensure runner's security.

Here is a simple usage:

```
runcfg['trace'] = True
runcfg['calls'] = [1, 2, 3, 4] # system calls that could be used by testing programs
runcfg['files'] = {'/etc/ld.so.cache': 0} # open flag permitted (value is the flags of open)
```
