## Lab 02

- Name: Ashbal khan
- Email: khan.174@wright.edu

Instructions for this lab: https://pattonsgirl.github.io/CEG2350/Labs/Lab02/Instructions.html

## Part 1 Answers

Full / absolute path to your private key file: 

Command to SSH to AWS instance:
```
[Place your ssh command here]
```

## Part 2 Answers

1. `chmod u+r bubbles.txt`
    - Means: Adds read permission for the file owner (user). Group and others remain unchanged. 
    - Assessment: Good. The file’s owner should normally have read access, and there is no security concern here.
2. `chmod u=rw,g-w,o-x banana.cabana`
    - Means: Sets the owner’s permissions to read and write only. Removes write permission from the group. Removes execute permission from others.
    - Assessment: Good. Since this file is data, not a program, it should not be executable. Limiting group and others from modifying it helps prevent unwanted changes.
3. `chmod a=w snow.md`
    - Means: Gives write-only permission to all users (owner, group, and others). This removes both read and execute permissions.
    - Assessment: Bad. A Markdown file should be readable, not write-only. With this setting, no one can open the file to see its contents, which makes it useless.
4. `chmod 751 program`
    - Means: Owner has full permissions (read, write, execute). Group has read and execute. Others have execute only.
    - Assessment: Good for an executable file. The owner can manage it, the group can inspect and run it, and others can only run it without changing it.
5. `chmod -R ug+w share`
    - Means: Recursively adds write permission for both the owner and the group on the `share` directory and everything inside it. This means the owner and group members can create, edit, or delete files inside the directory.
    - Assessment: Good for collaboration. Members of the group can share work. The caution is that everyone in the group can also delete or overwrite files, so group membership should be controlled.

## Part 3 Answers

1. Command to create new user: `sudo adduser ashba` 
2. Path to new user's home directory: `/home/ashba`
3. Evaluate if `ubuntu` can add files to new user's home directory: No. When trying `touch /home/ashba/test_from_ubuntu`, the system returned "Permission denied". The directory `/home/ashba` is owned by `ashba` and has permissions `drwx------` (700), so only the owner can create files inside.
4. Command to switch to new user: `su - ashba`
5. Command(s) to go to new user's home directory: After switching, `pwd` showed `/home/ashba`.
6. Evaluate if new user can add files to user's home directory: Yes. As `ashba`, I ran `touch test_from_ashba.txt` and it succeeded. Since `ashba` owns `/home/ashba` and the directory has 700 permissions, the owner has full control.
7. Command to return to `ubuntu` user: `exit`
8. Command to return to `ubuntu` home directory: `cd ~`  
   - Verified with `pwd` → `/home/ubuntu`

## Part 4 Answers

1. Command(s) to create group named `squad` and add members: `sudo addgroup squad`
2. Command(s) to add `ubuntu` & user to group `squad`: `sudo usermod -aG squad ubuntu`
   - `sudo usermod -aG squad ashba`
3. Command(s) to allow `squad` to view the `ubuntu` user's home directory contents: - `sudo chgrp squad /home/ubuntu`
   - `sudo chmod 750 /home/ubuntu`
   - This ensures the owner has full access and members of `squad` can list and enter `/home/ubuntu`.
4. Command(s) to modify `share` to have group ownership of `squad`:  - `sudo chgrp -R squad /home/ubuntu/share`
   - `sudo chmod -R 2775 /home/ubuntu/share`
   - The `2` sets the setgid bit so new files inside `share` inherit the `squad` group automatically.
5. Describe your tests and commands with the user account:  - Switched to `ashba` with `su - ashba`.
   - Ran `ls /home/ubuntu` and was able to view contents because of group `squad` and 750 permissions.
   - Inside `/home/ubuntu/share`, created a file using `echo "hello" > file_from_ashba.txt`. The file appeared with group `squad`.
6. Describe the full set of permissions / settings that enable the user to make edits:  - `/home/ubuntu` is group-owned by `squad` with 750 permissions, letting `ashba` enter and view the directory.
   - `/home/ubuntu/share` is group-owned by `squad` with 2775 permissions, giving owner and group full read/write/execute access.  
   - The setgid bit keeps all new files inside `share` under group `squad`, so both `ubuntu` and `ashba` can collaborate.

## Part 5 Answers

For each, write the command used or answer the question posed.

1. Command(s) to make file using `sudo`: `sudo touch madewithsudo.txt` 
2. Command(s) to make file with `root`:  - `sudo -i`
   - `touch /home/ubuntu/madewithroot.txt`
   - `exit`
3. Describe / compare ownership and permissions of files:  - Both `madewithsudo.txt` and `madewithroot.txt` were owned by `root:root` by default with mode `rw-r--r--`.
   - That means only root can write; ubuntu and ashba could only read until permissions were changed.
4. Which account can do what actions? (Type Y or N in columns)

Contents inside of `share`
| Account   | Can View  | Can Edit  | Can Change Permissions    |
| ---       | ---       | ---       | ---                       |
| `root`    |Y          | Y         |Y                          |
| `ubuntu`  | Y         |  Y        |Y                          |
| `BOB`     |  Y        |  Y        |N                          |

`madewithsudo.txt`
| Account   | Can View  | Can Edit  | Can Change Permissions    |
| ---       | ---       | ---       | ---                       |
| `root`    |Y          |Y          | Y                         |
| `ubuntu`  |Y          |N          | N                         |
| `BOB`     |Y          | N         | N                         |

5. Command(s) to modify permissions: `sudo chown ubuntu:squad /home/ubuntu/madewithsudo.txt`
6. How to give user account `sudo`:  - `sudo usermod -aG sudo ashba`
   - Verified with `su - ashba` and `sudo -l`.

## Citations

To add citations, provide the site and a summary of what it assisted you with.  If generative AI was used, include which generative AI system was used and what prompt(s) you fed it.

