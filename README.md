# README for LO_roms_user

### This repo is a place for user versions of code used to compile ROMS code associated with the git repository LO_roms_source.

 KMH LO_roms_user
 
    This repo is a place for user versions of code used to compile ROMS code associated with the git repository LO_roms_source_git.

    see https://github.com/parkermac/LO_roms_user for more


# upwelling 
Test case and followed directions on Parker's readme

# x11b 
Notes on steps and edits made to code s/t I can run the forecast on Klone.

**(1) On my github and personal machine**

In my github, I created a fork to Parker's LO_roms_user.  

* *I used github instead of Terminal/apogee, so could get most recent version of code. Probably doesn't matter for x11b, but did matter in upwelling test case*  

* It is renamed in my github and machine "Parker_LO_roms_user" 

I then made a copy of Parker's x11b in my LO_roms_user.  

While in the forked parker_LO_roms_user (on my local machine) do this: 

```
cp -r x11b ../LO_roms_user/
```
***Don't use mv because that will suggest changes to Parker's LO_roms_user. You can't undo a move, but putting the file back will set gitdesktop back to no changed files :) ***

Then made the following edits:
##build_roms.sh 
- `build_roms.sh` : edited line 173 parker --> kmhewett

commit changes to LO_roms_user to github. Then pull on apogee + Klone



