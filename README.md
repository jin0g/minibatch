# minibatch

This repository contains three shell scripts to simplify running parallel processes on a system with one or more GPUs. The scripts are:

1. parallel: runs a specified command in parallel a certain number of times.
2. allocuda: assigns an available GPU to a specified command and runs it.
3. runjobs: continuously reads commands from a file and runs them one by one.


## Scripts

### parallel

The parallel script executes a provided command in parallel a certain number of times.

Usage:

```bash
$ ./parallel <times> <command> [args...]
```

### allocuda

The allocuda script executes a provided command with a specified GPU assigned to it. The script will continuously loop over the available GPUs and once it finds an available one, it will assign it to the command and run it.

Usage:

```bash
$ ./allocuda <command> [args...]
```

### runjobs

The runjobs script continuously reads commands from a specified command file and runs them one by one.

Usage:

```
$ ./runjobs <command_file>
```

## Usage Example

Suppose you have a script called train.py that trains a machine learning model and takes an argument for the model's name. If you want to train the model with different names on each GPU in parallel, you can use the three scripts together.

First, create a command file with each line being a train.py command with a different model name. For example, commands.txt could look like this:

```bash
python train.py model1
python train.py model2
python train.py model3
python train.py model4
python train.py model5
```

Then, you could use the parallel, allocuda, and runjobs commands together as follows:

```
$ parallel 2 allocuda runjobs commands.txt
```

This command runs two instances of the runjobs script on commands.txt in parallel, each assigned to a different GPU by the allocuda script.

Each instance of the runjobs script continuously picks a command from commands.txt and executes it. Since allocuda assigns an available GPU to each command, this setup allows you to train multiple models in parallel on different GPUs.
