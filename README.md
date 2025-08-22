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

**(3) Make sure we have the information required to run the forecast** 

***(3A) Grid***  
On Klone, under gscratch/macc/LO_data/grids, do:

```
cp -r /gscratch/macc/parker/LO_data/grids/cas7 ./
```
could have scp'd from apogee too.   


***(3B) LO/dot_in***  
On Klone (git pull under my LO). To make sure we have the most uptodate version

**(4) Forecast scenarios**   
***(4a) Forcing was created but forecast didn't run; forcing files are on klone already (get_forcing == False).***  
One solution:  
   * On klone: copy forcing files from parker's LOo/forcing/ to kmhewett's for: f2025.08.21 (.22 and .23). This avoided making changes to get_lo_info and/or driver_roms4, but not sure if best. 
   
   * <span style="color: pink;">*TODO: REMOVE THIS STEP: On apogee: copy last history file from /dat1/parker/LO_roms/cas7_t1_x11b/f2025.08.20/ocean_his_0025.nc to kmhewett's*<span>

   * Grab prior day for restart  *(Note example forecast to start on 2025.08.21)*   
   Copy files from Parker's LO_roms.  
   under gscratch/macc/kmhewett/LO_roms/cas7_t1_x11b/f2025.08.20, From Parker's folder copy:  
      * ocean_his_0025.nc  
      * klone_batch.sh *(edit parker --> kmhewett)*  
      * bio_Banas.in  
      * liveocean.in  *(edit parker --> kmhewett)*  
      * log.txt (might not be needed, but is helpful for me)

   * On klone, under gscratch/macc/kmhewett/LO, do:
   ```
   LOd="/gscratch/macc/kmhewett/LO/driver"
   $LOd/driver_roms4.py -g cas7 -t t1 -x x11b -r forecast -s continuation --get_forcing False --group_choice macc --cpu_choice compute -np 200 -N 40 < /dev/null > $LOd/forecast_t02.log&
   ```

   *The forecast_t02.log was my testing numbering. forecast_t01.log has the errors from my first attempt to run the forecast, and t02 was my second attempt. <span style="color: red;">TODO: Curious if I need to use Parker's naming for logs in the future, in which case I'd call this: `cas7_forecast.log` </span>*  


<span style="color: green;"> **Repeat for nested grids (south puget sound and wgh) (xn11b):**</span>

# xn11b
(see above steps, repeating for nested grid; numbers are out of order below but match x11b)  
**(1) pull and update xn11b files**   
On personal computer, 

```
cp -r xn11b ../LO_roms_user/
```
Then made the following edits:
- `build_roms.sh` : edited line 173 parker --> kmhewett

commit changes to LO_roms_user to github. Then pull on apogee + Klone

**(3) Make sure we have the information required to run the forecast**   
***(3A) Grid***  
On Klone, under gscratch/macc/LO_data/grids, do:

```
cp -r /gscratch/macc/parker/LO_data/grids/wgh2 ./
cp -r /gscratch/macc/parker/LO_data/grids/oly2 ./
```
could have scp'd from apogee too.   

***(3B) LO/dot_in***  
On Klone (git pull under my LO). To make sure we have the most uptodate version

**(2) Compile x11b**  
While on klone in the directory LO_roms_user/xn11b, do these steps, waiting for each to finish, to compile ROMS:  

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

**(4) Forecast scenarios**   
***(4a) Forcing was created but forecast didn't run; forcing files are on klone already (get_forcing == False).***  

   * grab forcing for forecast days from Parker's dir to my dir for oly2 and wgh

   * Grab prior day for restart  *(Note example forecast to start on 2025.08.21)*   
   Copy files from Parker's LO_roms.  
   under gscratch/macc/kmhewett/LO_roms/oly2_t1_xn11b/f2025.08.20 [repeat for wgh2_t1_xn11b] From Parker's folder copy:  
      * ocean_his_0025.nc  
      * klone_batch.sh *(edit parker --> kmhewett)*  
      * bio_Banas.in  
      * liveocean.in  *(edit parker --> kmhewett)*  
      * log.txt (might not be needed, but is helpful for me)

```
python3 $LOd/driver_roms4.py -g wgh2 -t t1 -x xn11b -r forecast -s continuation --get_forcing False --group_choice macc --cpu_choice compute -np 200 -N 40 < /dev/null > $LOd/wgh2_forecast_02.log  

python3 $LOd/driver_roms4.py -g oly2 -t t1 -x xn11b -r forecast -s continuation --get_forcing False --group_choice macc --cpu_choice compute -np 200 -N 40 < /dev/null > $LOd/oly2_forecast.log&
```