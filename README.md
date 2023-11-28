# OpenPBS User Guide

This is as condensed a possible. For more details see [References](##References).

## PBS Commands

| Command    | Action                                |
|------------|---------------------------------------|
| `qsub`     | submit a job                          |
| `pbsnodes` | get info about hosts or vnodes        |
| `qstat`    | display jobs, queues, and server info |
| `qalter`   | alter a job                           |
| `qdel`     | deletes jobs                          |

See details with `man <command>`.

## Creating a PBS Script

A PBS job script consists of the following:

- a shell specification
- PBS directives
- Job tasks (programs or commands)

```bash
#!/usr/bin/env bash
#PBS -N hello.sh
#PBS -l walltime=1:00:00
#PBS -l ncpus=3
echo "Hello from: $(hostname)" > hello.sh.out
```

The example requests one hour of time, 3 CPUs and executes the commands. `#PBS ...` is a PBS directive. They are optional. PBS will use the script name as the job name by default.

### Additional Scripting Notes

- A PBS script's variable `$0` will not be the script name. 
- A PBS script starts in the execution node's user's `home` directory.
- A scripts `stderr` and `stdout` can be saved by either of the following:
  - during submission: `qsub -keo script.sh` or by PBS directive `#PBS -keo`, they are output to job's the _exec_vnode_'s `home` directory.
  - you could also write to a log in the script itself.

## Submitting a PBS Job

Submission

- `qsub hello.sh`

Returns

- `<sequence number>.<server name>` 

You'll need the job identifier for any actions involving the job, such as checking job status, modifying the job, tracking the job, or deleting the job.

## Useful command strings

1. List jobs by user.

    ```bash
    qstat -u <user_name>
    ```

2. List detailed job info for a specific job.

    ```bash
    qstat -f <job_id>
    ```

3. List detailed job info for a specific job and filter the output.

    ```bash
    qstat -f <job_id> | grep -e 'Job Id' -e 'Job_Name' -e 'Job_Owner' -e 'exec_vnode'
    ```

------------------------------

## References

- [PBS User Guide](./docs/pbs_userguide.pdf)
