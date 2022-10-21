# backup a directory bash script
## Files in this folder:

**backupd.sh** :it is the bash script that contains the code of the backup project
      
                no changes could be done to this script

**makefile:** it is the file that when called with make in terminal it class the backupd.sh to be executed 

               you can edit the first 4 lines to specify the arguments you want to give to the bash script

**backupdir** : it is an example of directory that can hold the backups 
            
               you can use any other directory but edit its name in the makefile first

**directory1:** it is an example of a directory that could be backed up

               you can use any other directory but edit its name first in the makefile

## Code explanation

## makefile

the make file has 3 targets that run when the **make** command due to the **all:** command presented in the beginning of the make file. The 3 targets are:

#### 1. generate
which creates a new directory for backups if it does not exit 

the flag **-p** in **mkdir** command is the reason that no error occurs if the backup directory already exists

#### 2. executable
which makes the bash script executable because scripts in default are not executable unless **chmod** command is used
#### 3. bash 
which runs the bash script and passes its arguments which was initialized in the beginning of the makefile
## backupd.sh
### 1.Parameters validation

1. It is validated that the arguments entering the script are 4 exactly not more or less

2.It is validated that the interval_secs and the max_backups are positive integers as it checks if all values of string are in the range from 0 to 9 without any letters or 
 negative through the following code

    number='^[0-9]+$'
    if ! [[ $interval_secs =~ $number ]] ; then
	    echo "you must enter +ve integer for interval seconds"
	    exit
    fi


3.it checks that the source directory already exists 
 
     if ! [[ -e $dir ]] 
      # -e condition checks if a directory or file exists
      #if the above condition is true then the file does not exist


if any error was detected an error message would be printed and the script would exit

### 2.The initial code executed at the very beginning
1.the script saves the information of the source directory in a file named directory-info.last

2.the code copies the dir in a directory in the backup directory whose name is the current date

       cp -r $dir $backupdir/$x
      # -r flag is used to copy recursively (copy the contents of each sub directory)

current date is calculated through the following command
 
           x=$(date +%F-%H-%M-%S)
           #date : the command that returns the current date
           #+% :means a format 
           #the %F prints the date in the following form YYYY-MM-DD
           #%H return current hour
           #%M returns the current minutes
           #%S returns the current seconds

### 3.The code that will execute for infinity
1.Sleep is called to wait for the interval_secs specified by the user

2.Saves the current information of the directory in the file directory-info.new

3.compares directory-info.last with directory-info.new so that if any modification occurs this test will return false else it would return true

    if ! cmp -s "directory-info.new"  "directory-info.last" ;
    #this cmp instruction compares the content of these 2 files 

4.In case a change occurs:

loop counts the number of directories in the backup directory and also determines the oldest one 

          sub_directories=($backupdir/*)
	 	oldest=${sub_directories[0]}
	 	for folder in $backupdir/*;do
	 	   count=`expr $count + 1 `
	 	   if [[ $folder -ot $oldest ]]; then
	 	     oldest=$folder
	 	   fi
	 	done
 explanation of how the oldest sub-directory is found, through the next instruction we compare the subdirectory with the previous one in the sub directories array and the $oldest variable is updated when condition is met
            
          if[[ FILE1 -ot FILE2]]
          #which returns true if FILE1 is older than FILE2

then we check if the number of backups reached its maximum 
we would remove the oldest backup that we found
         
the directory-info.last is now updated with the value in directory-info.new
 
either we reached maximum backups or not when we reach this point it means we have the ability to create new directory in the backup directory

## Installation

you need to download make

Open a new terminal and run the following command

```bash
make install
```

## Steps to run the project

1.After extracting the folder that contains this readme file,open the make file

2.In the make file : the first 4 lines there are predefined arguments to be passed to the bash script you can change any of the values if you want from after the equal sign 

**the first argument** is the source directory you want to backup

**the second argument** is the destination in which backups would be created

**the third argument** is the the interval between each check on the source directory measured in seconds measured in seconds

**the fourth argument** is the max number of backups you need to keep in the backupdir

3.save the make file

4.In the project's folder right click then choose open terminal then type **make** and click enter




