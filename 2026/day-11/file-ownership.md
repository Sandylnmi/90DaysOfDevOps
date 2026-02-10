# File Ownership Challenge (chown & chgrp)

* Change file owner using chown
* Change file group using chgrp

<p>In Linux, every file and directory is assigned an owner (a specific user) and a group (a collection of users), which together determine who can access and modify the file based on a set of permissions (read, write, and execute).</p> 

## The Owner
<p>The owner is the specific user account that controls the file or directory.</p>

* Default: Typically, the user who creates the file is automatically assigned as its owner.
* Control: The owner (or the root user) can change the file's permissions using the chmod command and even transfer ownership to another user using the chown command.
* Permissions: The owner has their own set of permissions that apply only to them, separate from the group and all other users. 

## The Group
<p>A group is a logical collection of users who share common access rights to files and system resources. This simplifies access management in multi-user environments.</p>

* Purpose: Instead of assigning permissions to individual users, an administrator can add multiple users to a group, and then grant the group specific permissions to a file or directory.
* Membership: A user can belong to multiple groups, including a primary group and secondary (supplementary) groups.
* Permissions: Any user who is a member of the file's assigned group will have the permissions defined for that group.

<img width="481" height="153" alt="image" src="https://github.com/user-attachments/assets/221e8576-7d91-46ee-9fff-bebefc5b05ca" />

## Basic chown Operations

<img width="452" height="142" alt="image" src="https://github.com/user-attachments/assets/a07cd9f3-3fb0-49d4-a875-84e60b745d8a" />

## Basic chgrp Operations

<img width="502" height="129" alt="image" src="https://github.com/user-attachments/assets/8a5d79ad-ea69-4024-a2e3-430baebe40ee" />

## Combined Owner & Group Change

Using `chown` you can change both owner and group together:

1. Create file `project-config.yaml`
2. Change owner to `professor` AND group to `heist-team` (one command)
3. Create directory `app-logs/`
4. Change its owner to `berlin` and group to `heist-team`

**Syntax:** `sudo chown owner:group filename`

<img width="574" height="171" alt="image" src="https://github.com/user-attachments/assets/01c0a562-304f-4380-abe8-d56d2cba85f5" />

<img width="544" height="158" alt="image" src="https://github.com/user-attachments/assets/40bcbdf7-924f-4a5a-8a63-b7a8d4d04acd" />

## Recursive Ownership

1. Create directory structure:
   ```
   mkdir -p heist-project/vault
   mkdir -p heist-project/plans
   touch heist-project/vault/gold.txt
   touch heist-project/plans/strategy.conf
   ```

2. Create group `planners`: `sudo groupadd planners`

3. Change ownership of entire `heist-project/` directory:
   - Owner: `professor`
   - Group: `planners`
   - Use recursive flag (`-R`)

4. Verify all files and subdirectories changed: `ls -lR heist-project/`

<img width="528" height="592" alt="image" src="https://github.com/user-attachments/assets/e0e40b90-e691-44c9-9f6c-5fbd768de401" />

## Practice Challenge

1. Create users: `tokyo`, `berlin`, `nairobi` (if not already created)
2. Create groups: `vault-team`, `tech-team`
3. Create directory: `bank-heist/`
4. Create 3 files inside:
   ```
   touch bank-heist/access-codes.txt
   touch bank-heist/blueprints.pdf
   touch bank-heist/escape-plan.txt
   ```

5. Set different ownership:
   - `access-codes.txt` → owner: `tokyo`, group: `vault-team`
   - `blueprints.pdf` → owner: `berlin`, group: `tech-team`
   - `escape-plan.txt` → owner: `nairobi`, group: `vault-team`

**Verify:** `ls -l bank-heist/`

<img width="441" height="127" alt="image" src="https://github.com/user-attachments/assets/8bf41be6-6441-49a1-becb-e30bcea86bf1" />
<img width="615" height="215" alt="image" src="https://github.com/user-attachments/assets/99c26e1f-e88a-402e-970a-f8fd44308a92" />

<hr/>

## Commands

- View ownership : `ls -l filename`
- Change owner only : `sudo chown newowner filename`
- Change group only : `sudo chgrp newgroup filename`
- Change both owner and group : `sudo chown owner:group filename`
- Recursive change (directories) : `sudo chown -R owner:group directory/`
- Change only group with chown : `sudo chown :groupname filename` 

