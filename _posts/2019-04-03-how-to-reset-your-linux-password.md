# How to reset you linux password

Ever forgot your login password of you linux machine? We will there is simple solution for that which involves three simple steps

1. [Boot into recovery mode](#boot-into-recovery-mode)
2. [Find the username (optional if you already know yours)](#find-username)
3. [Change the password (Important step!!)](#changing-the-password)

### Note
 There might various linux flavours available but all of those are built with linux so these steps would be applicable 

## Boot into recovery mode
- If you machine has dual os you might automatically get the bootloader else you have manually go to the bootloader during the boot time
- To manually go to the bootloader press and hold <kbd>Shift</kbd> (or) <kbd>Esc</kbd>

- Once bootloader opens every linux os installed will give you two option 
    + OS to be loaded 
    + Advanced Option
- Select advanced options which takes you to the next screen again leaving you with two options 
    + OS to be loaded
    + Recovery Mode

- Select recovery mode to enter into recovery mode 
- In the recovery options select ```root```
- After selecting ```root``` if recovery password is set it will ask for the password else press <kbd>Enter</kbd>

## Find Username 
- In Linux the user name will be the name of the home folder
- Hence after entering the recovery terminal use the following command
    ```
    ls /home
    ```
- The user name will be displayed below

## Changing the password

- After you have the username of which password has to be changed you have to remount the disk with write permissions because in recovery the disk will be in read-only mode

- Use the following command to remount the disk with write permissions
    ```
    mount -rw -o remount /
    ```
    this will remount the root
- After mounting the root use the following command to reset the password
    ```
    passwd <username>
    ```
    replace username with your username
- It will ask you to enter new password and retype the password
- The password will be updated, type ```exit``` to exit recovery and select ```resume``` option in recovery option to go back to normal boot



Hope this help you! Cheers</:)>!


