Notes and scripts on getting started working on a ML project utilizing a remote machine.

# Setup
Install the following  on the local machine:
* lysncd
* Docker (optional)

Install the following  on the remote machine:
* tmux 
* Nvidia drivers
* Docker (with Nvidia gpu support)

# Workflow
An example workflow might be:
1. Start lsyncd to sync local files to the remote, `lsyncd ./scripts/lysncd_config`.
2. Do some work on local.
3. ssh into remote.
4. Create new tmux session with `tmux`, or attach to existing session with `tmux a -t 0` (for example).
5. Run code on remote, for example `./scripts/runjupyterlab`.
6. From the local machine, commit changes to version control.
7. Repeat steps 2-6.

The local machine can be turned off while letting the tmux session continue on
the remote. 

# Port forwarding
If you start a web service on the remote, such as Jupyter Lab, you can access
the service from the local machine by ssh port forwarding. For example:
```
ssh -N -f -L localhost:8888:8888 john@remote_machine
```
The ssh flags have the following behaviour:
* `-f` makes the command run in the background. It means that
   you can continue using the same terminal for other work. It is optional.
* `-N` no command is to be executed on the remote.
* `-L` this is the actual port forwarding flag.

# Data
The setup assumes that the remote machine uses input data from `data` 
directory and stores output in `out` directory, and that the amount of data in
the folders is too much to also be stored on the local machine. The lsyncd 
config file specifically avoids sending data to these directories. Any data you
wish copy back to the local machine must be done manually. Note that lsyncd is
one way (local -> remote) and if you include the `out` or `data` directories in 
the sync, then any data created on the remote might be deleted by lsyncd.



