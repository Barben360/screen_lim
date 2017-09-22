# screen_lim by Barben360

## Created 09-22-2017 - Confirmed working on Debian 9 Stretch

When you are doing intensive simulation on large sets of inputs using your CPU, you want all its cores to be used. Several possibilities are then possible:
- Your code is multi-threaded (e.g, using OpenMP) and you can then run sequentially all your simulations and all the cores will be used. Best choice ! But not always that easy to code efficiently and fast.
- Your code is not multi-threaded, and the only way to run your tests using all your cores is to run several simulations at a time. If you have an X-core processors, you can either:
	- Run all your simulations in the same time. For this, you will for instance run as many instances of screen as you have simulations. The CPU time will be shared between your execution instances, and you will globally lose performance. Moreover, you will potentially spend a lot of RAM, and if you want to evaluate the time performance of your simulation, the numbers you'll get won't be representative at all since you don't control how your CPU splits its time among running programs.
	- Manually run as many simulations as you have cores, and wait for one to be finished, staying in front of your PC, to run a new one. You will loose time monitoring manually, you will loose time between two executions. However, the evaluation of time performance you can make will be representative.
	- You just run everything sequentially. This is the simplest way, but your simulations will be X times longer.
	
The Python script proposed here simply allows you to run all your instances of simulations in the same time, but only X of them will be allowed to be started in the same time. Therefore, if you have 100 programs to run and a 4-core processor, only the 4 first will start, one on each core. When the first of them will be finished, number 5 will start, then number 6, etc.
This way, all your cores will be fully used 100% of the time, while each CPU core doesn't have to be shared between several programs. Therefore, you will be able to time your simulations with no interference.

## Prerequisites:

- Python 3
- screen

## How to setup the script:
Just put it in your path... what else?

## How to use the script:
Here is the help
```
usage: screen_lim [-h] [-n NAME] [-m MAX_SCREENS] [-t WAIT_TIME] [-M] command

Runs command in background screen when a computation core is available. Be
careful: this command is blocking, meaning the remaining of the program it is
called in will not run until a screen instance has been able to be be
launched. Note: only the "command" argument is necessary.

positional arguments:
  command               Command to be run in background screen.

optional arguments:
  -h, --help            show this help message and exit
  -n NAME, --name NAME  Fancy name of the screen to create.
  -m MAX_SCREENS, --max_screens MAX_SCREENS
                        Force number of maximum screens to be run in the same
                        time. By default, the number of cores of the CPU is
                        used.
  -t WAIT_TIME, --wait_time WAIT_TIME
                        Force the time between two attempts of starting a
                        screen instance (in seconds)
  -M, --major           Adds 1 available slot to the default (or forced one,
                        but then it is useless). This is useful if the command
                        is run from a screen which is not supposed to be
                        counted in the total number of screens (low impact on
                        processor, only to manage other screens).
```

So for instance, let's suppose you want to run an instance of a simulation started by a script "simulation.sh". Then you just have to run:
```
screen_lim "simulation.sh"
```
The number of cores of your computer is automatically resolved so no worries.

Now if you want to run the same script, give the associated screen a fancy name "SIM", and wait for 2 seconds only (instead of 5 by default) between two attempts of running a same script, and if you want 1 core in your 4-core processor to be left unused (so to use only 3 cores):
```
screen_lim "simulation.sh" -n "SIM" -m 3 -t 5.0
```

All you have to do to run your 100 simulations is running this line 100 times in a script, applied to all your simulations.
You can choose the same fancy name for all your simulations, that won't be an issue.

Hope you'll enjoy !
