# State-Feeder
An automatic logging system for recording the durations for which a generator operates and maintaining the logs in a database. This system involves a raspberry pi unit with an operating system installed within to enable interaction with the user, a voltage step down regulator for enabling the GPIO pins within the raspberry pi unit to be connected to the generator power supply, a storage device for maintaining the logs, and a power backup for powering up the unit. This system allows the user to have easy access to the running times of the generator in a tabular format and automates the process of calculation of fuel consumption and estimation for future fuel needs.


# HARDWARE COMPONENTS

The Automatic Logging System involves the use of following hardware components.
1. A Raspberry Pi Unit : A raspberry pi is a small general purpose computer capable of performing tasks that a general desktop computer would do, when used along with a keyboard, a mouse and a desktop. A raspberry pi 3B+(used in this project) within itself contains following components.
1a. A processor
1b. 1 Gigabyte of Random Access Memory
1c.  Networking : dual-band IEEE 802.11b/g/n/ac WiFi, Bluetooth   4.2, and Gigabit Ethernet
1d.  GPU supporting upto 1920X1200 WUXGA resolution
1e. General purpose input-output (GPIO) connector with 40-pin pinout

2. A voltage step down adapter : The General Purpose Input Output pins of the raspberry pi unit are connected to the regular 230V Alternating Current supply of the generator via a voltage step down regulator to get the supported 3.3V Direct Current supply in order to enable the GPIO pins to detect the high and low outputs.
3. A Storage Device : A storage device, in form of a micro SD card is used in the current version of the project. The storage space is required for installing the operating system to enable interaction with the user, as well as providing storage for the logs to be maintained in form of a database. This is also where the scripts of codes that enable the working of the system are stored.
4. Power Backup : In order to power up the device a power bank is provided in the current version of the product that connects to the raspberry pi unit through the micro USB port provided on the raspberry pi via the use of a USB to micro USB cable. The power bank in turn can be recharged with the use of a general purpose smartphone charger with micro USB output cable.

# SOFTWARE

1. Operating System : At the core of the system is the Raspbian Operating System installed on to the raspberry pi. This operating system is based on linux and provides user with ability to interact with the raspberry pi.
2. Python Interpreter: The core program of the invention is implemented in the python language and run using the python interpreter pre-installed on the raspbian operating system. Apart from this following packages are needed to be imported into the program to enable proper functioning of the device.
2a. RPi.GPIO : Used for communicating with the GPIO pins in the raspberry pi, in this case, for reading input as high or low.
2b. xlsxwriter : for creating new excel spreadsheets
2c. openpyxl : for loading excel spreadsheets for editing
2d. shutil : for copying data from one spreadsheet to another
2e. os: for reading the directories of local storage to see if the spreadsheet of particular name is present
3. Excel Spreadsheet: Excel spreadsheets are used to store the data regarding the timings in a tabular format and enable user easy access to the data. These excel files can easily be read across various devices with different operating systems using various softwares. 



WORKING
Once  the device is connected to the generator, and the raspberry pi is powered on, the device starts taking input from the General Purpose Input Output pins. The pins are connected to the power source via a voltage step down regulator which regulates the voltage of the power source from the general 230V Alternating Current to a 3.3V Direct Current to enable the GPIO pins to read inputs without suffering damage. When the generator switch is turned on, i.e. the generator is being used, the GPIO pins receive a high signal and when the switch is turned off the GPIO pins receive a low signal. The high signal is treated as 1 or True in Boolean value and the low signal is treated as 0 or False in Boolean value. 
A main python program drives the entire system. It aims at maintaining separate excel spreadsheets for each day and storing each day’s log in the respective file. A default format for initial information is maintained in a separate file and it’s content is copied to each new spreadsheet created before the data from the device is entered. The basic format of the spreadsheet includes the following attributes – Refernece number, Date, On Time, Off Time and Total Time.
First, the python program imports the RPi.GPIO, openpyxl, shutil, xlsxwriter and os modules. The RPi.GPIO module provides a class for controlling the GPIO pins and thus helps in detecting the high and low signals in the GPIO pins. The program now waits until the GPIO receive a high signal, indicating that the generator has been powered up. On receiving the signal the program checks if the excel spreadsheet for today was already created using the imported package os. If not, a new file for today’s entries is created using the imported package xlsxwriter and the default content is copied from a separately maintained spreadsheet using the imported package shutil. 
The python program uses the imported module time to get the current date and time. On encountering the high signal in the GPIO pin, and successfully ensuring that the respective spreadsheet has been created, the program puts the current time as an entry under the On Time attribute. Also the current date is put under the Date Attribute. The spreadsheet file is saved and the program waits until a low signal is encountered on the GPIO pins, indicating the generator is turned off again. At this moment the current time is noted and an entry for off time is put under the Off Time attribute. The file is saved again. 
The entire process is repeated infinitely and the program keeps running in the background forever. Entries are made for each switch presses. At every night at 00:00 the spreadsheet for current date is saved and closed and a new file is created corresponding to the next date.


