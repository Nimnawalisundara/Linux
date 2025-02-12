# Assingment 6 - APT

## Part 1: Understanding APT & System Updates 

1. Check system’s APT version:
    - apt --version
        
    ![](/img/Assignment6/1.PNG)

2. Update the package list:
    - sudo apt update

    ![](/img/Assignment6/2.PNG)

***Important of this command***
- It updates the local package database with the latest versions available from the configured repositories.
- It ensures you have access to the latest security patches and bug fixes.
- System can correctly handle dependencies when installing or upgrading software.

3. Upgrade installed packages:
    - sudo apt upgrade -y

***difference between update and upgrade***
- apt update - No packages are installed, removed, or upgraded. It only updates the metadata about the packages.
- apt upgrade - Installs newer versions of the packages currently installed on the system. It does not remove or install new packages if dependencies have changed.
- apt update - It refreshes the information about available packages and their versions in the repositories.
- apt upgrade - It compares the installed package versions with the updated package lists and upgrades the installed packages if newer versions are available.

4. View pending updates (if any):
    - apt list --upgradable

    ![](/img/Assignment6/3.PNG)

    ***no any pending updates***

## Part 2: Installing & Managing Packages

5. Search for a package using APT:
    - apt search image editor

    ![](/img/Assignment6/4.PNG)

    package- koko/noble 23.08.5+ds.1-1ubuntu2 amd64 (Image gallery for Plasma mobile)

6. View package details:
    - apt show koko

    ![](/img/Assignment6/6.PNG)

***dependencies***
- koko-data (= 23.08.5+ds.1-1ubuntu2)
- libkf5purpose5
- qml-module-org-kde-kquickcontrolsaddons
- qml-module-org-kde-kquickimageeditor 
- qml-module-org-kde-qqc2breezestyle | qml-module-org-kde-qqc2desktopstyle
- kio
- libc6 (>= 2.34)
- libgcc-s1 (>= 3.3.1)
- libkf5configcore5 (>= 4.98.0)
- libkf5configgui5 (>= 5.74.0)
- libkf5coreaddons5 (>= 5.53.1)
- libkf5dbusaddons5 (>= 4.99.0)
- libkf5guiaddons-bin, libkf5guiaddons5 (>= 4.96.0)
- libkf5i18n5 (>= 5.17.0)
- libkf5kiocore5 (>= 5.92.0)
- libkf5kiowidgets5 (>= 5.59.0)
- libkf5notifications5 (>= 4.96.0)
- libkf5windowsystem5 (>= 5.91.0)
- libkokocommon0.0.1 (>= 23.08.3)
- libqt5core5t64 (>= 5.15.1)
- libqt5dbus5t64 (>= 5.0.2)
- libqt5gui5t64 (>= 5.14.1) | libqt5gui5-gles (>= 5.14.1)
- libqt5positioning5 (>= 5.6.0)
- libqt5qml5 (>= 5.14.1)
- libqt5quick5 (>= 5.4.0) | libqt5quick5-gles (>= 5.4.0)
- libqt5sql5t64 (>= 5.0.2)
- libqt5svg5 (>= 5.15.1)
- libqt5widgets5t64 (>= 5.0.2)
- libqt5x11extras5 (>= 5.6.0)
- libstdc++6 (>= 13.1)
- libxcb1

7. Install the package:
    - sudo apt install koko -y

    package successfully installed. Verified  by launching the application.

8. Check installed package version:
    - apt list --installed | grep koko

    ![](/img/Assignment6/8.PNG)

    - Version: 23.08.5+ds.1-1ubuntu2

## Part 3: Removing & Cleaning Packages

9. Uninstall the package:
    - sudo apt remove koko -y

    - This command remove the specified package. But keeps the configuration files.

10. Remove configuration files as well:
    - sudo apt purge koko -y

    ***difference between remove and purge***

    - Remove: Deletes the package but keeps configuration files.
    - Purge: Deletes the package and its configuration files. 

11. Clear unnecessary package dependencies:
    - sudo apt autoremove -y

***Important of this step***
    - Cleans Up Unused Dependencies.
    - Frees Up Disk Space.
    - Improves Performance.
    - Reduces Security Risks.

12. Clean up downloaded package files:
    - sudo apt clean

    ![](/img/Assignment6/12.PNG)

    - Clears Cache.
    - Clean Slate: It's helpful when you want to start fresh with package management without any remnants of previously downloaded packages.
    - Frees Up Disk Space.

## Part 4: Managing Repositories & Troubleshooting

13. List all APT repositories:
    - cat /etc/apt/sources.list

    ![](/img/Assignment6/13.PNG)

    ***Notice in this file***
    - The file includes different components or sections of the repository
    - Contains security updates repositories.
    - Contain the ubuntu repository.

14. Add a new repository
    - sudo add-apt-repository universe
    - sudo apt update

    ![](/img/Assignment6/14.PNG)

    ***Types of packages are found in the universe repository***

    - Community-maintained open-source software packages.
    - Text Editors: Editors.
    - Productivity Software: Office applications, such as word processors, spreadsheet programs.
    - Security Tools: Software for encryption, network security, and privacy protection.

15. Simulate an installation failure and troubleshoot:
    - sudo apt install fakepackage
    I got this error message- E: Unable to locate package fakepackage.

    ![](/img/Assignment6/15.PNG)

-usually means that the package you're trying to install isn't available in current repositories or there might be a typo in the package name.
- steps.
    1. Update package lists
        - sudo apt update
    2. Check package name
        - apt search <package-name>
    3. Enable extra repositories
        - sudo add-apt-repository universe  
        - sudo apt update
    4. Try a different mirror
    5. Use snap as an alternative
        - sudo snap install <package-name>

## Bonus Challenge (Optional):

- Use apt-mark to hold and unhold a package so it doesn't get updated.
    - sudo apt-mark hold vim
    - sudo apt-mark unhold vim

    ![](/img/Assignment6/16.PNG)

    ***Why would you want to hold a package?***
    - Prevent breaking changes – If a newer version has bugs or compatibility issues.
    - Avoid dependency conflicts – When an update might break other installed software.
    - Stick to a specific version – If you need a stable version for your workflow.
    - Protect custom configurations – Some updates might overwrite critical config files.

