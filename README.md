# README for LO_roms_user

### This repo is a place for user versions of code used to compile ROMS code associated with the git repository LO_roms_source.

 KMH LO_roms_user
 
    This repo is a place for user versions of code used to compile ROMS code associated with the git repository LO_roms_source_git.

    see https://github.com/parkermac/LO_roms_user for more


# upwelling 
Test case and followed directions on Parker's readme

# x11b 
Notes on steps and edits made to code s/t I can run the forecast on Klone.

**(1) pull and update x11b files**   

In my github, I created a fork to Parker's LO_roms_user.  

* *I used github instead of Terminal/apogee, so could get most recent version of code. Probably doesn't matter for x11b, but did matter in upwelling test case*  

* It is renamed in my github and machine "Parker_LO_roms_user" 

I then made a copy of Parker's x11b in my LO_roms_user.  

While in the forked parker_LO_roms_user (on my local machine) do this: 

```
cp -r x11b ../LO_roms_user/
```
***Don't use mv because that will suggest changes to Parker's LO_roms_user.*** *You can't undo a move, but putting the file back will set gitdesktop back to no changed files :)*

Then made the following edits:
- `build_roms.sh` : edited line 173 parker --> kmhewett

commit changes to LO_roms_user to github. Then pull on apogee + Klone

**(2) Compile x11b**  
(see https://github.com/parkermac/LO_roms_user/tree/main for more details on compiling ROMS on klone).

While on klone in the directory LO_roms_user/x11b, do these steps, waiting for each to finish, to compile ROMS:  

```
srun -p compute -A macc --pty bash -l
```
```
module load intel/oneAPI
```
```
./build_roms.sh -j 10 < /dev/null > bld.log &
```
*when finished compliling*

```
logout
```

**(3) Forecast scenarios**   

**(3a) Forcing was created but forecast didn't run; forcing files are on klone already (get_forcing == False).**  
One solution:  
   * On klone: copy forcing files from parker's LOo/forcing/ to kmhewett's for: f2025.08.21 (.22 and .23). This avoided making changes to get_lo_info and/or driver_roms3, but not sure if best. 
   
   * On apogee: copy last history file from /dat1/parker/LO_roms/cas7_t1_x11b/f2025.08.20/ocean_his_0025.nc to kmhewett's

   * On klone, do 
   ```
   LOd="/gscratch/macc/kmhewett/LO/driver"
   python3 $LOd/driver_roms3.py -g cas7 -t t0 -x x4b -0 2025.08.21 -r forecast -np 200 -N 40 --get_forcing False < /dev/null > $LOd/forecast_t01.log &
   ```