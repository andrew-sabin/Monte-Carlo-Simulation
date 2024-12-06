# Monte-Carlo-Simulation

Based on this [video](https://www.youtube.com/watch?v=_kVH8Wh7tBo) the projects aim to simulate how many golfballs going down a ski jump will potentially make it into the hole.

Assisted with the help of Professor Mike Bailey at Oregon State University.

## General Math and Random Generation

### Random Generation
Here the math will contain four constant variables:

**BEFORE_Y**- the top of the ski jump.
**AFTER_Y**- the end of the ski jump.
**DIST_X**- the distance between the ski jump and the golf hole.
**RADIUS** - the golf hole radius that exists at the horizontal distance_x.

Because we don't have exact measurements we need to also have the following for the tolerances:

**BEFORE_YDY**- the tolerance value of the BEFORE_Y value.
**AFTER_YDY**- the tolerance value of the AFTER_Y value.
**DIST_XDX** - the tolerance value of the DIST_X value.

These are all used with calculation with the **Ranf()** function that will fill the **BeforeY**, **AfterY**, and **DistX** arrays with random values that are calculated like the following:

```
float Ranf (float low, float high){
  float r = (float) rand();
  float t = r / (float) RAND_MAX;

  return low + t * (high - low);
}
```
Where the values low and high are placed into the function like so:
```
Ranf(BEFOREY - BEFOREYDY, BEFOREY + BEFOREYDY);
```

The TimeOfDaySeed() function is used to make sure the values that are stored within the arrays are consistently random with each value.

### Mathematical Formula
_vx_ - Horizontal Velocity of the ball coming off the ski jump.
_t_ - Time to reach the group from the ski jump.
_dx_ - Horizontal distance traveled by the ball before landing on the ground.

![alt text](https://raw.githubusercontent.com/andrew-sabin/Monte-Carlo-Simulation/refs/heads/main/GolfBallEqn.png)

Golf Equations Provided by Mike Bailey of Oregon State University.

### End Result

With both projects, the values will either be printed onto a CSV file (an .err file in the case of the graphics card) or onto the console with the following results representing each column:

**Number of Threads**, **Number of Trials**, **Probability**, and **Maximum Performance (MegaTrials Per Second)**. 


## Project 1- OpenMP Monte Carlo Simulation
Project 1 uses OpenMP in order to do multithreaded parallelism on different cores with a processor.

**NUMT** - Number of threads being used
**NUMTRIALS** - Number of trials.

We set the number of threads to be used with the code, where NUMT represents the amount of threads used in the program is NUMT.
```
omp_set_num_threads( NUMT );
```

Then the pragma is set for the for-loop used for each monte carlo trial:

```
#pragma omp parallel for default(none) shared(BeforeY, AfterY, DistX, stderr) reduction(+:numSuccesses)
                for( int n = 0; n < NUMTRIALS; n++ )
```
Here we are setting omp to make all the for loops run parallel with one another, having no default for safety reasons, and the BeforeY, AfterY, and the DistX arrays being shared along with the stderr with in each thread.

Loop Bash file used to create the multiple results:
```
#!/bin/bash
for t in 1 2 4 6 8
do
  for n in 1 10 100 1000 10000 100000 500000 1000000
  do
     g++   proj01.cpp  -DNUMT=$t -DNUMTRIALS=$n  -o proj01  -lm  -fopenmp
    ./proj01
  done
done
```

### Results of the Monte Carlo Trials
The results can be see in the [csv file](https://github.com/andrew-sabin/Monte-Carlo-Simulation/blob/main/proj01/proj01.csv).

And the pdf report.

## Project 5- CUDA Core Monte Carlo Simulation
Project 5 uses CUDA Cores in order to do parallelism on different cores with a graphics card or graphics card server.
