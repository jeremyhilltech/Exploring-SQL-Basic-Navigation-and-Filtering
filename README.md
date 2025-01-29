# Exploring-SQL-Basic-Navigation-and-Filtering

<p align="center">
<a href="https://cdn.pixabay.com/photo/2017/09/28/22/43/database-search-2797375_640.png"><img src="https://cdn.pixabay.com/photo/2017/09/28/22/43/database-search-2797375_640.png" title="tux" /></a>
</p>

# Managing File and Directory Permissions in Linux
This lab serves as an exploration of permissions on directories and files within Linux. We will check permissions to ensure that each is properly configured for their respective `user`, `group`, or `other` owners. The point here is to get a taste of the Principle of Least Privilege, which states that only the minimum amount of access for files and directories should be granted for the completion of necessary tasks related to the use of the data. 

## Context:

This lab is constructed from a learning module in the Google Cybersecurity Professional course. The shell was provided via remote access in a browser, and therefore this lab is not easily recreated, as your system will not have some of the files and directories shown below. Work shown here is for demonstration and educational purposes only. 

For a list of commands used in this lab, please see the end of the lab. For more information on these commands and their options, please refer to the 'man' page for each of them inside your linux bash shell. Some items in this lab are highlighted as inline code for ease of reading and understanding the text even though `not everything written that way is actually a code or command`. Screenshots are provided which show all actual commands being used. 

## Scenario:
You are a security professional at a large organization. You mainly work with their research team. Part of your job is to ensure users on this team are authorized with the appropriate permissions. This helps keep the system secure. Your task is to examine existing permissions on the file system. You’ll need to determine if the permissions match the authorization that should be given. If they do not match, you’ll need to modify the permissions to authorize the appropriate users and remove any unauthorized access.

---
## Check File and Directory Details

<a href="https://imgur.com/8s6z6U2"><img src="https://i.imgur.com/8s6z6U2.jpg" title="LC1.1" /></a>

1. We begin by establishing who we are using the `whoami` command, which returns that we are `researcher2`. 
2. Next we need to know where we are, so we use the `pwd` command, which returns our working directory as `/home/researcher2`. 
3. We will be working in the projects directory, which we locate using the `ls -a` command to confirm its presence from our current working directory. 
4. We then use the `cd projects` command to drop into that location. 
5. Our instructions are to enumerate all of the files and directories within the projects directory and investigate permissions on several files. Here we see the returned information from the system using the `ls -la` command in order to include any hidden files in our current working directory. Below is the result, where we can see all permissions. 

<a href="https://imgur.com/1Valzqd"><img src="https://i.imgur.com/1Valzqd.jpg" title="LC1.2" /></a>

---
## The Permissions String

In the above image note that there is a series of 10 character strings that begin each line. These are permission strings, and they tell us what permissions are enabled on the given file or directory, as well as who has been given these levels of access. 

In this section, we’ll first discuss the possible characters and what each means. Next we'll discuss the types of owners of files in linux, and then we will tie that into the permissions string so we can use it as a tool to understand what the permissions are on files and directories. 

As you may note on observation, each string consists of a series of letters or hyphens. The characters used are as follows:

* A `d` at the beginning of a string represents the permissions for a directory
* A `-` (hyphen) at the beginning of a string represents the permissions for a file. If located within the string, the hyphen means that the permission has been revoked. 
* An `r` means that `read` permissions are granted. 
* A `w` means that `write` permissions are granted.
* An `x` means that `execute` permissions are granted. 

Next let’s observe that there are three types of owners in Linux:

* `user` - the owner or creator of the file. 
* `group` - every user is part of some type of group. 
* `other` - this is considered to be all other users on the system. 

From here we tie all of this information to the string itself. A useful way of conceptualizing permission strings is to think of them as 1 + 3 + 3 + 3. In this case, the first character represents whether we are talking about a directory or file (`d` or `-` respectively), and the rest are “triplets” which pertain to one of three types of owners, that is `users`, `groups`, and `others`. In other words, characters 2-4 denote `user` permissions, 5-7 denote `group` permissions, and 8-10 denote `other` permissions.

If all permissions were enabled for everyone, that would end up looking like: `-rwxrwxrwx`

Please note that this is an example of what is known as "global permissions" and is a critical security risk. 

In the example `-rwxrwxrwx`, the hyphen in position 1 tells us we’re working with the permissions of a file. The first “triplet” of characters represent `user` permissions, the second triplet represents `group` permissions, and the last triplet represents `other` permissions. Now let’s take a look at our results again: 

<a href="https://imgur.com/rdtmKvG"><img src="https://i.imgur.com/rdtmKvG.jpg" title="LC1.2" /></a>

If we take a look at the `drafts` directory, we see the permissions: `drwx--x---`
Read from left to right, we confirm we’re working with a directory (`d`). We can see that `users` have read, write, and execute privileges, `group`s only have execute privileges, and `others` have no privileges. 

---
## Changing File Permissions

Now that we understand what we’re looking at, we’re going to change the permissions on one of these files. In order to do this, we’re going to use the `chmod` command, which will require that we first enter which permissions we want to edit, and specify the file or directory in which to implement those changes. Let’s learn how to do that first. 

When using `chmod`, we tie each owner type to an argurment of what to do with their permissions:

`+` adds a permission

`-` removes a permission

`=` overwrites all existing permissions with the given permission

Once we have decided how and who to assign our permissions to, we end the command with the specific file or directory to which the permissions will be applied. 

Consider the following example: `chmod g+w,o-r access.txt`

Using `chmod` we must take into consideration that `u` = user, `g` = group, and `o` = others. As you’d expect, it follows that `r` =  read, `w` = write, and `x` = execute. Knowing this, we can deduce that this command is telling `chmod` to give group the write permission, and to remove others from the read permission on the file called `access.txt`. Said more literally, it might read this way: “chmod - group add write, others remove read - on access.txt file.” While this isn’t correct, it gives you an idea of the instruction you’re giving the computer. Please note that for each permission you edit, you *must* insert a comma between each set of changes, and you ***cannot*** put a space after the commas. Now that we understand `chmod`, let’s look at our files again. 

Let’s say that our organization does not allow `others` to have write access to *any* files. If we look closely, we can see that `project_k.txt` is currently configured incorrectly. To fix this, we’ll use the `chmod` command to alter the permissions for `others` on this file. 

<a href="https://imgur.com/ag1Sr7a"><img src="https://i.imgur.com/ag1Sr7a.jpg" title="LC2.1" /></a>

Highlighted we can see that this file does allow `others` to write to the file. Next, we run our `chmod` command and tell it to remove the write permission from `others`: 

`chmod o-w project_k.txt` yields the following result:

<a href="https://imgur.com/p2E6i7h"><img src="https://i.imgur.com/p2E6i7h.jpg" title="source: imgur.com" /></a>

Seen above, we have removed the write permission from `others` on project_k.txt now, and that is reflected by running our `ls -la` command to check the current permissions in the directory. 

---
## Change File Permissions on a Hidden File

You may have noticed that by using the `ls -la` command to view the files in this directory, we have been listing a file that begins with a `.`. The `-la` argument tells the system to show us all of the files, including hidden ones. Hidden files and directories begin with `.` in the file system. Now let’s say that `.project_x.txt` is an archived file, which is why it’s been hidden from the normal list view. Our company policy is that no one is to have write permissions for archived files, and only `users` and `groups` may have read permissions. Given this, we can see that this hidden file is misconfigured, as it currently allows for `user` and `group` to write to it, however `groups` are not able to read it.

<a href="https://imgur.com/UJGLrwQ"><img src="https://i.imgur.com/UJGLrwQ.jpg" title="LC3.1" /></a>

Let's change that by running `chmod u-w,g-w,g+r .project_x.txt` and we should see the following:

<a href="https://imgur.com/TKtoIBe"><img src="https://i.imgur.com/TKtoIBe.jpg" title="LC3.2" /></a>

As you can see, we have now correctly configured .project_x.txt to be readable only by `users` and `groups`. 

---
## Change Directory Permissions

In Linux, it's actually the `x` (execute) permission that allows a given owner the ability to see or access a directory. Let's say that we discover that our `drafts` directory has incorrect permissions enabled. Company policy states that only our user account (`researcher2`) has permission to access this directory. Our permissions tell us that currently, `groups` are also able to access this directory:

<a href="https://imgur.com/2PAKxHe"><img src="https://i.imgur.com/2PAKxHe.jpg" title="LC4.1" /></a>

In order to correct this, we have to do two things. First, we have to make sure that we are currently in the parent directory to `drafts`, in this case the `projects` directory. (Use `cd` to navigate to `projects` if needed.)

From there we can run `chmod g-x drafts` to remove `group` access to the drafts directory:

<a href="https://imgur.com/erNn46N"><img src="https://i.imgur.com/erNn46N.jpg" title="LC4.2" /></a>

And we can now confirm that `researcher2` is the only user allowed to access the `drafts` directory now, in compliance with company policy. 

---
## Summary

In completing this lab we have learned how to identify ourselves on a linux command line, navigate to a target directory and confirm our location, and list all of the files and directories we want to see, including hidden files. We've configured the permissions on these files and directories in accordance with company policy and reduced our attack surface in doing so. Only those owners who require access to these files now have access, and unnecessary permissions have been revoked where needed. In essense, we've strengthened one of the essential parts of the CIA Triad in our organization, namely Confidentiality. This is why regular audits are important; we must confirm that we aren't leaving holes in our attack surface to both internal and external threats. The Principle of Least Privilege requires that we only give permissions where absolutely necessary for employees to do their jobs, and of course to keep all unauthorized connections from outside the organzation from accessing our data. 

---
## Commands Used in this Lab:
* `whoami` = tells you what user you are currently operating as in the system
* `ls` = list all items in the working directory, argument `-l` shows permissions, argument `-la` shows permissions and hidden files. 
* `pwd` = print the working directory
* `cd` = change directory
* `chmod` = (change mode) = changes file/directory permissions for users, groups, and others




