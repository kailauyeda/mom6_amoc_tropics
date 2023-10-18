# MOM6 Terminal Cheat Sheet
This cheat sheet was created to take notes as I learn how to compile and run MOM6 from the command line. Much of it is summarized documentation from the [MOM6 Documentation]((https://mom6.readthedocs.io/en/main/api/generated/pages/Diagnostics.html))

# Table of Contents
1. [MOM6 File Structure](File-Structure)
2. [Diag_Table](Diag_Table)
3. [Restart vs New Run](Restart-vs-New-Run)
4. [MOM_parameter]()


# File Structure

# Diag_Table
To understand the data output file naming conventions, we first need to know how files are named.
## Naming Files
Files are named in the ```diag_table``` File section at the top.

eg: 
```
"MOM Experiment"
1 1 1 0 0 0
"prog_%4yr_%3dy", 30, "days", 1, "days", "Time", 365, "days"
```
If we want to change the name of the output file, we must also change the file name that will be pulled in corresponding field section. 

eg:

```
"ocean model" , "u", "u", "prog_%4yr_%3dy", "all", .false., "none", 2
```
Note that the ``` prog_%4yr_$3dy ``` matches for both lines of code. A more detailed description for the rest of the lines of the code can be found on the [MOM6 Documentation Website](https://mom6.readthedocs.io/en/main/api/generated/pages/Diagnostics.html). 

## Understanding the Line 
This section will break down what the name of the file tells us, based on the information specified in ```diag_table```.

Below is a breakdown of the meaning of each term in the line (again, this is taken from the [MOM6 documentation](https://mom6.readthedocs.io/en/main/api/generated/pages/Diagnostics.html))

>- ``` "prog_%4yr_%3dy" ``` &rarr; file name \
``` 30 ``` &rarr; how often output data is recorded and saved to file (data will be recorded approximately once a month)\
``` "days" ``` &rarr; specifies the units of the above frequency \
``` 1 ``` &rarr; specifies output is in netcdf file format \
``` "days" ``` &rarr; units for time axis \
``` "Time" ``` &rarr; name of time axis \
``` 365 ``` &rarr; a new file will be made every 365 days (each output contains 12 months worth of data) \
``` "days" ``` &rarr; ??

## Understanding the Name
Most of the output files are named as follows:\
```prog__0001_006.nc```\
```cont__0001_003.nc```\
```ave_prog__0001_003.nc```

prog indicates ---
cont indicates ---
ave_prog indicates ---

>- The first set of numbers specifies the year of the first day data is recorded. 
>- The second set of numbers specifies the day of the first day data is recorded. 
>- This format does not specify the month of the first day data is recorded. 

eg: The above discussed file line is in our diag_table. We run the model for 2.5 years (913 days). Here is what we would expect:

> > - We will get 2 output files since 2 years were completed (2 sets of 365 days)
> > - Each file will contain 12 time entries (12 sets of 30 days in one year)
> > - The first prog file will contain ```0001_031``` because the first recorded day of data is 30 days after the beginning of the simulation (1/01/01 + 30 days)
> > - The second prog file will contain ``` 0002_026``` because it is the 2nd year, and the first recorded day of data for that year is 1/26/02.

# Restart vs New Run
In the input.nm. file, there is the option for 
```ruby
&MOM_input_nml
  output_directory = './',
  input_filename ='n'
  restart_input_dir = 'INPUT/',
  restart_output_dir = 'RESTART/',
  parameter_filename = 'MOM_input',
                       'MOM_override' /
```
If `input_filename = 'n'`, then a new run will be started. This would include spin-up time in the time that we told the simulation to run for. 

On the other hand, if `input_filename = 'r'`, then the simulation will "restart." So, if we last finished the simulation at 60 days, then the next run completed will start using data from the 60th day. The model is able to store information about the 60th day data, as it creates a restart file that is stored in `restart_output_dir = 'RESTART/'`.
