## Assignment-4

***Task***

1. Create the Tupu user using the adduser script:
    - sudo adduser tupu

    ![](/img/Assignment3/1.PNG)

2. Create the Lupu user using the useradd command. Try to create a user profile, home directory, and user group   similar to Tupu.
    - sudo useradd -m -d /home/lupu -s /bin/bash -G lupu lupu
        -m: Create the user's home directory.
        -d /home/lupu: Specify the home directory path.
        -s /bin/bash: Set the login shell to /bin/bash.
        -G lupu: Add the user to the lupu group.

    ![](/img/Assignment3/2.PNG)

3. Create the Hupu system user with the login shell set to /bin/false
    - sudo useradd --system --shell /bin/false hupu
        - --system: Create a system account.
        - --shell /bin/false: Set the login shell to /bin/false to prevent login.

![](/img/Assignment3/3.PNG)

4. Add the users Tupu and Lupu to the sudo users.
    - sudo visudo
    - Add the following lines:
        - tupu ALL=(ALL:ALL) ALL
        - lupu ALL=(ALL:ALL) ALL

    ![](/img/Assignment3/4.PNG)

    *Adding users to the sudo group:*
    - sudo usermod -aG sudo tupu
    - sudo usermod -aG sudo lupu

5. Create a directory /opt/projekti and add both users (Tupu and Lupu) as owners. Only Tupu and Lupu should have access to list files in the directory, read, and modify them.

    - First, create the projekti group and add both users to this group.
    - Then, create the /opt/projekti directory and set permissions.
    - Set the setgid bit to ensure new files inherit the group.

    ![](/img/Assignment3/5.PNG)
    ![](/img/Assignment3/6.PNG)