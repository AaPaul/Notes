<!-- #### Table contents -->
Manual
===================

[TOC]

### Introduction
 
#### Environment requirement
```
Python=2.7
mysql=8.0
Anaconda=4.7.2
```
I created the virtual environment of python by Anaconda and in this part I will provide two versions of the requirement of the environment which are for Anaconda and Pip relatively.
```
alembic==0.9.6
beautifulsoup4==4.6.0
blinker==1.4
certifi==2019.11.28
click==6.7
dominate==2.5.1
Flask==0.12.2
Flask-Bootstrap==3.3.7.1
Flask-DebugToolbar==0.11.0
Flask-Login==0.4.1
Flask-Migrate==2.1.1
Flask-SQLAlchemy==2.3.2
Flask-Testing==0.7.1
Flask-WTF==0.14.2
itsdangerous==0.24
Jinja2==2.11.2
Mako==1.0.7
MarkupSafe==1.0
MySQL-python==1.2.3
mysqlclient==1.3.12
pam==0.1.4
pbr==3.1.1
Pillow==2.3.0
pycrypto==2.6.1
python-dateutil==2.6.1
python-editor==1.0.3
six==1.11.0
SQLAlchemy==1.2.0
Unidecode==1.0.22
visitor==0.1.3
Werkzeug==0.14.1
WTForms==2.3.1
```

I suggest the Anaconda version which is more complete and easier to export.
##### Pip
###### How to import
pip install -r requirements_pip.txt

##### Anaconda
###### How to import 
```
conda create --name enviroment_name --file requirments.txt
``` 

### How to install the project
###### Step 1 Install MySQL 5.7 above
Set MySQL default character as UTF-8
```
sudo vim /etc/mysql/my.cnf
```
Insert the following two lines:
```
[mysqld]
character-set-server = utf8
```
###### Step 2 Install python 2.7

```sh
$ sudo apt-get install python-setuptools python-dev python-MySQLdb libssl-dev

# Anaconda version
conda create --name enviroment_name --file requirments.txt
```
###### Step 3 Install python libraries
```sh
# pip version
sudo pip install -r file/requirements.txt

# anaconda version
# We don't need to install the requirements again as it has been already installed at the previous step.
```

### How to update code

All what you need to do is run `sync.sh` script with root account.

**`Sync.sh` will do the following things:**
1. Change the website to production mode
3. Change the ownership of all files and folders to www-date
4. Restart apache2

**`Sync.sh` comes with the following constraints:**
1. Must run the script with root account.
2. Must run the script under c3pd folder.

### How to convert XLSX files to UTF8 text files
Make sure all files are in UTF-8 format
##### The first way to convert XLSX files to UTF text files
###### Step 1 Open xlsx file with microsoft Excel software

###### Step 2 Save as Text(Tab delimited)(*.txt)
You will see "Confirm Save As" dialogue, Click Yes, and Click Yes again at the following dialogue.
Don't select "Unicode Text(*.txt)" type. Somehow, it doesn't work very well.

###### Step 3 Put the txt files in the Linux, and use any software you like to convert txt file to UTF-8 format.
I use sublime text to save the text files as UTF8 format.

##### Another way to convert XLSX files to UTF text files
###### Step 1 Creat xlsx file

###### Step 2 run a command similar to this 
`xlsx2csv -d 'tab' -l '\n' $someXLSX dataFile.txt`
The varialbe 'someXLSX' is your xlsx file and 'dataFile.txt' is the text file created.

###### Step 3 check if it is utf-8
A command like this one: encoding=`file -bi dataFile.txt | cut -f 2 -d=`
Will store the encoding in the variable 'encoding'

###### Step 4 Change it to uft-8 if it is not
f=dataFile.txt
```
iconv -f $encoding -t utf-8 $f > $f.utf8
mv $f.utf8 $f
```
This code will change whatever the encoding was stored into utf-8

This is the process that is used in input_script.sh to take input as a xlsx file instead of a text file


### Making Sure the database is configured
It might be a good idea to create a separate user to use the database at some point.
What follows shows how to do that if it is decided that is a good idea,

###### Creating the user
```
mysql> CREATE USER 'dt_admin'@'localhost' IDENTIFIED BY 'dt2016';
Query OK, 0 rows affected (0.00 sec)
```
###### Granting permisions
```
mysql> GRANT ALL PRIVILEGES ON dreamteam_db . * TO 'dt_admin'@'localhost';
Query OK, 0 rows affected (0.00 sec)
```


**The following code is the stuff that sqlalchemy uses to determine what database to talk to**

The code below is what it is right now
```
  SQLALCHEMY_DATABASE_URI = 'mysql://root:mysql@localhost/pedigree2'
```
This code follows the template if you are using mysql
```
  SQLALCHEMY_DATABASE_URI = 'mysql://username:password@nameOfServerMakingTheSQLRequests/nameOfDatabase'
```
Here is some documentation from SQLALCHEMY http://flask-sqlalchemy.pocoo.org/2.3/config/

The nameOfServerMakingTheSQLRequests is usually localhost since that is where the code that makes changes to the database is usually executed. Input from the user is used to make these executions but the user does not
make them directly


### Adding new columns to the database

1. Add the column as an attribute in the appropiate model
if you are adding one to the cultivar model:
  That is in the `models.py` file on line 63. The line you need to add will go along with the ones listed below:
```
    growth_habit = db.Column(db.String(45))
    type = db.Column(db.String(45))
    use_class = db.Column(db.String(45))
    released_year_notes = db.Column(db.Text)
    card_id = db.Column(db.Integer, db.ForeignKey('card.id_card'))
```
  if you have not declared sql columns like that before here is the link to the sqlalchemy documentation explaining how to do that http://flask-sqlalchemy.pocoo.org/2.3/models/

2. if you want it be editable you have to add it to the forms. The line you have to enter looks like the ones below
```
    subtype = TextField("Subtype", validators=(validators.Optional(),))
    affiliation_of_breeders = TextAreaField("Affiliation Of Breeders", validators=(validators.Optional(),))
    characteristic = TextAreaField("Characteristic", validators=(validators.Optional(),))
    use_type = TextField("Use Type", validators=(validators.Optional(),))
```
  These forms were made with WTF forms. A link to their documentation is here https://flask-wtf.readthedocs.io/en/stable/

3. After that is done you have to use the Flask-migrate tool to update the actual database. A tutorial on useing flask-migrate can be found here http://www.patricksoftwareblog.com/relational-database-migrations-using-flask-migrate/


### Initialize the Database (example)

The first step we should do is to build the database. These are the main processing for that.

###### 1. Create the user named addProgram 
```
Create USER 'addProgram'@'localhost' IDENTIFIED BY 'c1234567'; (your password)
```  
NOTE: need to check whether the host is ‘localhost’. If NOT, update it. We need to do as follows. 
```
update user set host='localhost' where user='addProgram’from mysql.user;
```

###### 2. Create the DB named c3pd2TestingAddProgram
``` 
CREATE DATABASE c3pd2TestingAddProgram;
```

###### 3. Assign permissions
```
GRANT ALL PRIVILEGES ON c3pd2TestingAddProgram.* to 'addProgram'@'localhost'; 
FLUSH PRIVILEGES;
```

###### 4. Update password
```
USE mysql;
ALTER USER 'addProgram'@'localhost' IDENTIFIED WITH mysql_native_password by 'mettalica’;
FLUSH PRIVILEGES;
```

###### 5. Import data into the database
We can run *initialize_db.py* or the script named *add_delete_or_reinitialize.sh*.

### Add new data into the database (example)
In the project, there seems no function for adding data one by one while there is a script for adding new data by importing a data file, which is named *add_delete_or_reinitialize.sh*

The steps of adding new data show below. Note: we are supposed to enter nothing when we run this script.

```
user@whateve:/c3pd2$ cd DBManagementScripts
user@whateve:/c3pd2$ ./add_delete_or_reinitialize.sh
Enter 'y' to reinitialize the database or 'd' to run the delete script and anything else to add entries
 
Enter crop name (required):
barley
Please confirm that was 'barley' you entered. Enter 'y' for yes
y
Enter cultivar data file name (required):
dataFiles/BarleyTestXcel.xlsx
Please confirm that 'dataFiles/BarleyTestXcel.xlsx' was what you entered for the data file name. Enter 'y' for yes
y
Enter card folder or press enter if there is no card folder:
data
Please confirm that 'data' was what you entered for the card folder name (blank means no folder). Enter 'y' for yes
 
Ok the please enter the correct card folder.
dataFiles/barley_cultivar_cards/
Please confirm that 'dataFiles/barley_cultivar_cards/' was what you entered for the card folder name (blank means no folder). Enter 'y' for yes
y
Enetering the regular data
Cultivar: Pleides is already in the DB
 
/usr/local/lib/python2.7/dist-packages/sqlalchemy/engine/default.py:507: Warning: (1366L, u"Incorrect string value: '\\xEF\\xBC\\x8Chig...' for column 'performance' at row 1")
  cursor.execute(statement, parameters)
Calculating COP Data
 
Entering COP data
Elapsed time: 0 sec
user@whateve:/c3pd2$
```

Everything else should work so congrats you did it. P.S: I would make a backup database and test before you do something that cannot be reversed because you never know.

There is an warning because of an incorrect string in one of the card files. This does not seem to effect the data, but it is probably a problem with the way pandoc transfered the data into a text file. 
It can also be seen that if a cultivar is already in the DB it is skipped and the user is notified.

### Deleting the data (example)
It is very easy to remove all data. We only need to do is running the file named `removeAndRecreateDB.py`.
```
python removeAndRecreateDB.py
```

### The introduction of scripts
#### ./DBManagementScripts/add_delete_or_reinitialize.sh
This is the main script that asks the user whether they want to add, reinitialize, or delete. It then runs the whichever script does what the user wants. This script was created to make a nice easy script for the user to use to execute the various database management scripts.

##### Description
This script first prompts the user for what script they would like to run, and then it will execute it.

##### Improvements
This script could be improved upon by using a 'until' or 'while' loop to allow the user to keep running scripts until they signify they want to quit. This would work similarily to the while loop in `deleteCultivars.py` or the various 'until' loops used in the bash scripts to force the user to confirm information inputed is correct.

###### The Program
The first line just says where to find bash.

The next two lines, which are shown below, get the input from the user
```
echo "Enter 'y' to reinitialize the database or 'd' to run the delete script and anything else to add entries"
read userAnswer
```
The echo statement asks for the input and the read statement stores it in the variable 'userAnswer'.

The next part is the decision making process. It is a if elif else statement that checks the user input and executes the correct script given the user input. This uses the bash if statement syntax and that is covered in detail in this tutorial https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php

It is shown below:
```
if [[ "$userAnswer" == 'y' ]]
then
  ./initialize_database.sh
elif [[ "$userAnswer" == 'd' ]]
then
  ./delete_cultivars.sh
else
  ./input_script.sh
fi
```

##### Using the Program
This is the program that is meant to be run by the user.

to run it just enter the following into the command line.
```
./add_delete_or_reinitialize.sh
```
and then follow the prompts in the command line.

#### ./DBManagementScripts/calculate_COP_v1.0.pl
This is the perl script created by Frank to calculate the COPs of the cultivars in the database.

#### ./DBManagementScripts/delete_cultivars.sh
This is the bash script that will be run when the user wants to delete cultivars. This script was written to enable the deletion of crops while maintaining the correctness of the COP data.

##### Description
It is a bash script that runs a python script `deleteCultivars.py` to delete the cultivars and then it runs a perl script to re-calculate the COP data. After the COP data is in the text file created by the perl script the python script 'enter_COPs.py' will update the COP data.

##### The Program
The first line just specifies where the bash interpreter is.

The two lines below :
```
echo What crop type will you be deleting from?
read crop
```
find out what crop type entries will be deleted from.

The long block that follows :
```
echo "Please confirm that was '$crop' you entered. Enter 'y' for yes"
read userAnswer
until [[ "$userAnswer" == 'y' ]]
do
  echo "Ok the please enter the correct crop type."
  read crop

  echo "Please confirm that was '$crop' you entered. Enter 'y' for yes"
  read userAnswer
done
```
confirms with the user that the crop type they previously entered was the correct crop type. It will not stop asking to confirm and getting a new entry until the user confirms it is the correct crop.

The line below makes a knew file to store parent information that will be filled by `deleteCultivars.py`.
```
touch parentRelationFile.txt
```

The line after that calls `deleteCultivars.py`

The two lines below tell the user that the COPs are being calculated and then the perl program is called to calculate them.
```
echo Calculating COP Data
perl calculate_COP_v1.0.pl -fparentRelationFile.txt
```

The two lines after that tell the user COP data is being inputted and then calls the python script to input them. They are shown below.
```
echo Entering COP data
python enter_COPs.py
```

The last two lines just remove the files that were used to calculate and input the COPs.

##### Using the Script
This script can be used alone and it is also called by add_delete_or_reinitialize.sh

To use it by itself just type what is shown below into the command line
```
./delete_cultivars.sh
```

#### ./DBManagementScripts/initialize_database.sh
This is the bash script that lets the user reinitalize the database. This script was made to allow the to reinitalize the database using either existing text files or excel files. The text file portion was put there for backwards compatibility with the old way of initializing the database.

##### Description
The user first inputs whether they want to user text files or excel files to input the bulk of their data.

If they elected for text files then they will be prompted for the all the files and card folders. This way only deals with the three current crops (wheat, flax, and barley).

##### The Program
The very first line specifies where the bash interpreter can be found.

The next line clears the database and then recreates the tables empty, and that bit is shown below.
```
python removeAndRecreateDB.py
```

After that the user is asked to choose between excel files or text files, and those two lines are shown below.
```
echo "Do you want to use excel files or do you have the text file ready? (type 'text' for text file and 'xlsx' for excel file)"
read textOrExcel
```
The echo statement requests the information, and the read statement reads the response into the 'testOrExcel' variable.

##### If text is selected
The if statement then checks if the 'textOrExcel' variable is equal to 'text', and if it is then the code similar to the snippet below will be ran three times one for each crop.
```
echo "Please enter the main text file for Flax"
read flaxUserFile
until [ -f $flaxUserFile ]
do
  echo "Not a regular file. Enter cultivar data file name (required):"
  read flaxUserFile
done

echo "Please enter the card folder for flax"
read flaxCardFolder
until [[ -d $flaxCardFolder || $flaxCardFolder = '' ]]
do
  echo "Not a folder or a blank. Enter a valid folder of just press enter if there is no folder"
  read flaxCardFolder
done
```
This snippet first asks for the main text file and then reads the response into a variable. That response is tested to make sure it is a file and will run a loop till it is set to a file.

After the file is got it will then ask for the card folder and read the card folder into a variable. It will then check to make sure it is either blank or a directory, and if it is neither it will keep asking the user for a different folder until it is either blank or a valid folder.

This code is about the same for all three crops (except for variable names), so I'll just explain it once.

Next job is to input the data that the user indicated should be used. All of the data is inputed in similar to the code snippet shown below.
```
touch parentRelationFile.txt

python addToDatabase.py "Flax" $flaxUserFile $flaxCardFolder

echo "Calculating COP Data for flax"
perl calculate_COP_v1.0.pl -fparentRelationFile.txt

echo "Entering COP data for flax"
python enter_COPs.py

rm parentRelationFile.txt

rm parentRelationFile.txt_cop.txt
```
The first part of this snippet creates the file that will store the parent relation data that will be created by `addToDatabase.py` and then used by the perl program to calculate the COP data.

The next part runs `addToDatabase.py` with the crop's name, file, and card folder as input. This then adds all the data from that info to the DB.

After that the user is notified the COP data is being calculated and then the perl program is called to calculate it.

Then the user is notified the COP data is being inputed, and then 'enter_COPs.py' is called to enter them.

At the end of that snippet the two files used to input COP data are removed, to make sure they are clean for the next time.

##### If Excel files are selected
If the user indicates they want to use excel files to initialize the database then the following code will be ran.
```
quit=''
until [[ $quit = 'q' ]]
do
  ./input_script.sh

  echo "enter 'q' if there are no more entries to enter: "
  read quit
done
```
This code will run the bash script called 'input_script.sh' until such time the user indicates they want to quit by entering 'q' when prompted.

##### Using the program
This program can be run by the user or the user could use the 'add_delete_or_reinitialize.sh' to run it.

To run it by itself just type
```
./initialize_database.sh
```

#### ./DBManagementScripts/input_script.sh
This is the bash script that inputs the cultivar data to the database. This bash script was written to make it possible to specify a card folder and excel file to use as data input and have cultivars inputed that way.

##### Description
This script first creates files to be used by other scripts that will be called. Then it collects all the information needed to add entries to the database. It then calls `addToDatabase.py` to add all the entries to the database. After that the perl script is called to calculate all the COPs, and then `enter_COPs.py` takes that information and enters it into the database. Then the temporary files that were used are deleted to make sure they are clean for next time.

##### The Program
###### First Part of Program
The very first line just says where to find the bash interpreter.

The next two lines of code create two different files that are explained below.

dataFile.txt : This will be the file that will store the tab-delimited version of the excel file.

parentRelationFile.txt : This will be the file that will store the parent relation data created when `addToDatabase.py` is run. It will then be sent as input to the perl program which will use it to calculate the COP data.

###### Get Crop Name
The next step this program tackles is getting the crop name. This is done with the code shown below.
```
echo "Enter crop name (required): "
read cropName
echo "Please confirm that was '$cropName' you entered. Enter 'y' for yes"
read userAnswer
until [[ "$userAnswer" == 'y' ]]
do
  echo "Ok then please enter the correct crop type."
  read cropName

  echo "Please confirm that was '$cropName' you entered. Enter 'y' for yes"
  read userAnswer
done
```
We first get the crop name and then confirm with the user that it is correct. If it is then we move on, but if it is not we enter a 'until' loop that asks the user to enter the correct name and to confirm. We stay in that loop till they confirm that the crop was entered correctly.

###### Get Excel File Name
**Get and confirm the file with the user**
The next part is getting the excel file from the user. The first part of that is to get the file name and confirm it with the user.
 ```
 echo "Enter cultivar data file name (required):"
 read userFile

 echo "Please confirm that '$userFile' was what you entered for the data file name. Enter 'y' for yes"
 read userAnswer
 until [[ "$userAnswer" == 'y' ]]
 do
   echo "Ok the please enter the correct data file."
   read userFile

   echo "Please confirm that '$userFile' was what you entered for the data file name. Enter 'y' for yes"
   read userAnswer
 done
 ```
The first two lines of this gets the file from the user.

Then the next part confirms with the user that they did in fact mean to enter what they did. If they did not then they will enter the 'until' so they can re-enter the name and confirm that they entered what they wanted. The loop will not end till they confirm.

**Check if it was a valid file**
The next thing to do is to make sure that the entered a valid file.
```
until [[ -f $userFile && ! -z $userFile ]]
do
  echo "Not a regular file. Enter cultivar data file name (required):"
  read userFile

  #
  # check if that was the correct file
  #
  echo "Please confirm that '$userFile' was what you entered for the data file name. Enter 'y' for yes"
  read userAnswer
  until [[ "$userAnswer" == 'y' ]]
  do
    echo "Ok the please enter the correct data file."
    read userFile

    echo "Please confirm that '$userFile' was what you entered for the data file name. Enter 'y' for yes"
    read userAnswer
  done
done
```
The '-f' checks if it is a valid file and the '-z' checks if what was entered was an empty string.

If niether of those tests pass then the steps from **Get and confirm the file with the user** will be repeated.

###### Get the Card Folder
The method of getting the card folder is almost the same as getting the excel file. The main difference is different messages in the echo statements. Another difference is that we do not use '-f', but we do use '-d' since we are looking for a directory. We also use '' instead of -z, but the can be used interchangably anyways.

##### Using the script
This script is used in 'add_delete_or_reinitialize.sh', but it could also be used on its own.

To use it on its own enter the following in terminal and follow the prompts that were described in this document.
```
./input_script.sh
```

#### ./initialize_database.sh
This script was made to allow the to reinitalize the database using either existing text files or excel files. The text file portion was put there for backwards compatibility with the old way of initializing the database.

##### Description
The user first inputs whether they want to user text files or excel files to input the bulk of their data.

If they elected for text files then they will be prompted for the all the files and card folders. This way only deals with the three current crops (wheat, flax, and barley).

##### The Program
The very first line specifies where the bash interpreter can be found.
The next line clears the database and then recreates the tables empty, and that bit is shown below.
```
python removeAndRecreateDB.py
```

After that the user is asked to choose between excel files or text files, and those two lines are shown below.
```
echo "Do you want to use excel files or do you have the text file ready? (type 'text' for text file and 'xlsx' for excel file)"
read textOrExcel
```
The echo statement requests the information, and the read statement reads the response into the 'testOrExcel' variable.

##### If text is selected
The if statement then checks if the 'textOrExcel' variable is equal to 'text', and if it is then the code similar to the snippet below will be ran three times one for each crop.
```
echo "Please enter the main text file for Flax"
read flaxUserFile
until [ -f $flaxUserFile ]
do
  echo "Not a regular file. Enter cultivar data file name (required):"
  read flaxUserFile
done

echo "Please enter the card folder for flax"
read flaxCardFolder
until [[ -d $flaxCardFolder || $flaxCardFolder = '' ]]
do
  echo "Not a folder or a blank. Enter a valid folder of just press enter if there is no folder"
  read flaxCardFolder
done
```
This snippet first asks for the main text file and then reads the response into a variable. That response is tested to make sure it is a file and will run a loop till it is set to a file.

After the file is got it will then ask for the card folder and read the card folder into a variable. It will then check to make sure it is either blank or a directory, and if it is neither it will keep asking the user for a different folder until it is either blank or a valid folder.

This code is about the same for all three crops (except for variable names), so I'll just explain it once.

Next job is to input the data that the user indicated should be used. All of the data is inputed in similar to the code snippet shown below.
```
touch parentRelationFile.txt

python addToDatabase.py "Flax" $flaxUserFile $flaxCardFolder

echo "Calculating COP Data for flax"
perl calculate_COP_v1.0.pl -fparentRelationFile.txt

echo "Entering COP data for flax"
python enter_COPs.py

rm parentRelationFile.txt

rm parentRelationFile.txt_cop.txt
```
The first part of this snippet creates the file that will store the parent relation data that will be created by `addToDatabase.py` and then used by the perl program to calculate the COP data.

The next part runs `addToDatabase.py` with the crop's name, file, and card folder as input. This then adds all the data from that info to the DB.

After that the user is notified the COP data is being calculated and then the perl program is called to calculate it.

Then the user is notified the COP data is being inputed, and then 'enter_COPs.py' is called to enter them.

At the end of that snippet the two files used to input COP data are removed, to make sure they are clean for the next time.

##### If Excel files are selected
If the user indicates they want to use excel files to initialize the database then the following code will be ran.
```
quit=''
until [[ $quit = 'q' ]]
do
  ./input_script.sh

  echo "enter 'q' if there are no more entries to enter: "
  read quit
done
```
This code will run the bash script called 'input_script.sh' until such time the user indicates they want to quit by entering 'q' when prompted.

##### Using the program
This program can be run by the user or the user could use the 'add_delete_or_reinitialize.sh' to run it.

To run it by itself just type
```
./initialize_database.sh
```

#### ./DBManagementScripts/enter_COPs.py
This script was created to enter the COP data for the various scripts. It works closely with the perl program since what the perl program outputs is used to figure out what COPs are needed to be inputed.

##### First 7 lines
There are 4 lines of code very close to the start of the file (right now in the first 7 lines), and they make the imports are made from the main directory not from DBManagementScripts. This is needed because some of the needed imports are located directories outside of this directories path. I tried to go back a directory in the import but it did not work out well.

The 'testdir' variable is the current directory (this was first used for the unit-test directory). The 'srcdir' is the directory we want to be in. This long line 'sys.path.insert(0, os.path.abspath(os.path.join(testdir, srcdir)))' changes the directory to the one we want for the imports.

##### The imports
From app we have imported the models and db module. This is so that we can use the database effectivley. We import the entire 'input_functions.py' file so that we have all the needed input functions.

The following imports
```
import threading
import time
from sys import stdout
```
were required for the next first bit of the program that reports to the user how much time has went by since the program started.

##### Other things to note about imports
Frank has expressed an interest in making this program completly separate from everything else and if this happens than the first lines described before would be unnecessary as all the needed files would be inside the directory.
For this idea I think the biggest problem would be knowing the models since they are part of the app.


##### The Program
The following part, found at the start of the following
```
class bcolors:
  HEADER = '\033[95m'
  OKBLUE = '\033[94m'
  OKGREEN = '\033[92m'
  WARNING = '\033[93m'
  FAIL = '\033[91m'
  ENDC = '\033[0m'
  BOLD = '\033[1m'
  UNDERLINE = '\033[4m'



start_time = time.time()
working = True

def elapsedTime():
  while(working):
    elapse = time.time() - start_time
    minute = int(elapse / 60)
    second = int(elapse - minute * 60)
    elapse = str(second) + ' sec'
    if minute != 0:
      elapse = str(minute) + ' min, ' + elapse
    stdout.write('\rElapsed time: ' + bcolors.FAIL + elapse + bcolors.ENDC)
    stdout.flush()
    time.sleep(5)
  print ''

threading.Thread(target=elapsedTime).start()
```
Notifies the user how long the program was running for.

This next lines
```
addCoefficientFileToDB("parentRelationFile.txt_cop.txt")

db.session.commit()
```
Call the function 'addCoefficientFileToDB' from 'input_functions.py' and that function inputs the COPs. More on exactly how this is done can, as always, be found at 'input_functions.py'.

The last line just tells the timer to stop.

##### Using the script
Generally the script is just called by one of the bash scripts, but you can call it yourself in the command line.

You would run this line :
```
python enter_COPs.py
```
It will only work properly if 'parentRelationFile.txt_cop.txt' is in the format we expect, because of this you will probably run the perl program first.


#### Others
##### ./DBManagementScripts/dataFiles
This is a folder I made to contain all of the data I was inputting. It may be a good place for you to put your data.

##### ./DBManagementScripts/designMaterials
This is where I put all the material I created when designing this module. I have also included additional documentation on all the scripts. That additional documentation will be in files like `removeAndRecreatDB.md` for  `removeAndRecreatDB.py`. There will also be a readme inside of it.


### The description of the code
In this section, the main functions of each part will be introduced.

##### ./run.py
It is the entry of the whole project. To start the system, we only need to run this file.

##### ./config.py
It is the setting of the global variables, like the connection information of the database and the HOST, etc. In addition, it judges whether we are in the debug model or not.

##### ./initialize_db.py
It is the file for database initialization. This file is totally different from the script for initializing the database. It has complete path for reading the the data from txt files. We only need to run `python initialize_db.py` and the database can be create. Note: It only can be used for initialization of the database as there is no other function for adding or editing the database.

##### ./app/forms.py
In this file, there are kinds of Classes which is for specifying display information.
1. Log information and Login form
2. The form of editing cultivars including card fields
3. The form of only editing cultivars
4. *HelpAddForm* is for the help page adding new information

##### ./app/views.py
This file includes the definition of error pages and a ```after_request``` function to set the version of browser.

##### ./app/tools.py
It sets a series of functions for render and compress data or transfer the format of data.
1. *render* is for beautifying webpages with javascript
2. It includes a function for transferring the data format from table to JsonString in order to pass on the web page (for datatable.js)
3. another function is not used in the project.

##### ./app/models.py
This file includes all definations of tables in the database (DB). Therefore, there are totally 6 Classes corresponding the 6 tables in the DB.

1. cropType \
It stores the name of the type of crop. Examples include Flax, Wheat, Barely and that is all now but there are different crops. Each cultivar takes an entry from this table to signify what kind of crop that cultivar is.

2. Search \
It is used to aid in searching function. This stores the cropType and the name of the attribute in attribute. Options store the searchable items for each attribute.

3. Card \
It stores all information in card table. The id is included as foriegn key to its respective cultivar so that it's information is linked to that cultivar.

4. Cultivar \
It includes all attributes and some necessary functional methods. (**The graph showing on the webpage is generated based on some of these functions. some thing wrong here**)
    1. getCultivarByID \
    A method to read information from the DB by ID.

    2. getParentList (a recursive function, can it be optimized?) \
    Create a list of all the ancestors of a cultivar.

    3. getParentRelationData (return 3 lists, parents, ancestorList and coefficientPair of parents name?) what is that? \
    Obtain information regarding parent relations. This includes a list of all the ancestors/parents, the original ancestors (those who have no known parents), and list containing all the COP data of the cultivar's parents.

    4. getDirectChildrenList \
    Create a list of all the direct children of a cultivar. This means no grandchildren or great-grandchildren are included yet.

    5. _removeDuplicate \
    this function creates a list of cultivars with no None values and no duplicates.

    6. getChildrenRelationData \
    It obtains a list of all the decendants of a cultivar.

    7. _addAttributesFromAttrToCultivar \
    Obtain information regarding children relations. This includes a list of all the decendants, a list of all registered children, and list containing all the COP data of the cultivar's decendants.

5. Coefficient \
There is all information from Coefficient table
6. Help \
The information shows on the Help page.

##### ./app/\_\_init__.py
It an entry of the whole project. It initializes all objects (like database and app structure) and start the browser log and also can uncomment some annotation for debugging.

##### ./app/view/cultivar.py
This file includes all logic functions of the home page.
1. getImageURL \
It is the setting of path of figures shows on the web page

2. getValidAttrList \
It builds class Card for getting the information of the cultivar and they are showing on the web page. *allValidAttrs* is the list of all attributes of a clutivar in the DB (which is defined in the *./config.py* file).

3. getAttrForForm \
It is to build a form for showing the information of the corresponding cultivar and editing it.  What is not included in the list produced is functions, type (this is because the form class calls this diffently), anything we do not want the user to edit. See formFilterList for more details.

4. getJustCultivarAttr (for editting) \
Gets all the attrs for form just in the cultivar class. This is needed in two places for the edit view. It is needed to get the attrs to set the form when only the cultivar data is present, and it is also needed to set all the data in the cultivar table in both cases.

5. getJustCardAttr (for editting) \
It is just for getting all the attrs for form just in the card class.

6. generateTreeHTML (create the first node) \
It is the function to create the Tree which shows on the web page. And the *subTree ()* is for generating pedigree tree.

7. getSVG \
It can get the graph which illustartes the relationship among some cultivars.

8. cultivar \
It is used to display the information of corresponding cultivar requested on the webpage. 


9. theTrees \
It shows the pedigree tree on the main page in Home page.

10. editFullCultivarEntry \
It defines the page for editing the information of a cultivar.

##### ./app/db/DB.py
1. getOrCreateModel 
It can search the information from DB by specific attributes and return a searching model including the first data from the searching result, if the model is not existing then create a model with the corresponding type. (for example, models.cropType)
2. Createmodel

##### ./app/db/DBproxy.py

It gets the data from database, there is the detail of the searching operation. 

1. getSearchOptionList \
This functions gets all the search attributes for a particular entry from the Search table defined in `models.py`.

2. getAllCrops \
It gets all crops from the DB and return a list

3. getCultivarsByColumns 
(**figure out the meaning of rows and columns**)

4. getCultivars 
**figure out the meaning of rows and columns**

5. getSearchOptions \
This the logic of the filters which show in the upper-left of the Browser webpage.
Every time the system want to connect the database, we can just import the *DB_Proxy*.


##### ./app/db/input_functions.py
This file stores all operation functions for managing db, for example, the initialization of the db is based on these functions.
1. addCoefficientToDB \
Add coefficient to the DB

2. addCoefficientFileToDB \
It's an expand on addCoefficientToDB and it can be used for adding all coefficients

3. getCleanName \
It takes a name from file and modifies it to be in the format we want

4. addOption \
It adds an option to search table entry that was also passed in as a parameter, the detail can be seen at `initializeDB.py`

5. findKey \
It retrieves the key of a line

6. isInvalidLine \
It can judge if the follow sentence is valid, if not then skip it

7. getCultivarByName \
It can obtain the entry of Cultivar

8. getCultivarIDByName \
The way to get ID by Name of a cultivar


##### ./DBManagementScripts/addToDatabase.py 
This is the python script that actually adds the entries to the database. It is called by initialize_database.sh and input_script.sh for the purpose of adding entries. Most of the code that is run is functions from the input_functions.py file.

##### ./DBManagementScripts/deleteCultivars.py
This is the python script that enables the deletion of cultivars

##### ./DBManagementScripts/enter_COPs.py
This is the python script that enters the COP data

##### ./DBManagementScripts/removeAndRecreatDB.py
This is a python script that removes everthing in the database and then creates all the files

##### ./DBManagementScripts/removeAndRecreateDB.py
This is the python script for dropping all data in the dataset (reset file).