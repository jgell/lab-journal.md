# Learning Git, Hacking into nao robot

The text below is our first attempt at using the markdown syntax. ROCO222 Introduction to Sensors and Actuators

**This is an example of bold text**

*This is an example of italics*

~~Strikethrough~~

    Item 1
    Item 2 
    Item 3

Step 2 - Command-line 101

opened the terminal amd tried a few commands

$ ls

This command gives us a list of directories.

cd /tmp

This command changes directories, in this case to tmp.

cd $HOME

cd is used to change a directory.In this case, the home directory. The dollar sign means a normal user as opposed to a 'root' user.

mkdir

mkdir is for making new directories.

echo "hello" > hello.md

echo is a built in command that writes its arguments to standard form. This writes "Hello" in a markdown document named hello.md

cat hello.md

cat is short for concatenate. cat allows use to create single or multiple diles , view contents of a file, concatenate files and redirect the output in terminal or files. Using this command, the terminal ouput "Hello", thereby showing what we wrote in the markdown document.

cp hello.md hello-again.md

This copies the contents of hello.md into a new markdown named hello-again.md

mv hello-again.md hello-hello.md

This renames "hello-again.md" to "hello-hello.md"

rm hello.md

This has deleted the hello.md directory.

rm -rf the rm -rf

is the same as rm -r -f the -r meaning "remove directories and their contents recursively" the -f will "ignore nonexistent files and arguments, never prompt"

cat /proc/cpuinfo

This command lists characteristics of the computer's cpu, incluidng number of cores.

Commands on terminal: Refer to image 01-01
Step 3 - Github Account

An account has been made to push changes to a remote repository
Step 4 - Git repository

Showcasing git config commands: Refer to image 01-02
Step 5 – Your first commit

Git commit short command: Refer to image 01-03
Step 6 - Version Tracking

For this module, we have used git to create a lab journal using the markdown syntax. We have learned the basics on git repositories, including the master branch and practised making additional branches. The text in these branches can be merged with the master branch. We have also begun to commit changes to a repository, adding a description on the updates made. By using git push, we can upload our changes to a remote repository- in this case on our github account.
Step 7 - Going Social

Pushing the changes to Github remote repository: Refer to image 01-04
Part 2 - Hacking a robot

The task asked us to interface with a robot using the Linux terminal and some python code. The first step was to connect to the robot itself (named chapman). I used

ping chapman.local and then ssh nao@192.168.0.184

Refer to image 01-05 for terminal screenshots of this

I entered the required password and was now connected to the robot. The next step was to find a way to send it a message.

I created a python file (named hack4.py) in the terminal and copied the code given:

from naoqi import ALProxy tts = ALProxy("ALTextToSpeech", "localhost", 9559) tts.say("I've hacked you, robot!")

Python editing: Refer to image 01-06

Then I ran the executable hack4.py with:

nano hack4py

Running the python code: Refer to image 01-07

https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26220135_504131956668433_1698501814619730595_n.jpg?oh=f8871a015b0ab1e564601327dadceec7&oe=5AE8BD47
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26231353_504131536668475_3526629583152953852_n.jpg?oh=810e04a65d0e7d5f2b09cfd69a2d3efc&oe=5AF23AF2
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26733918_504131983335097_4004086473661975552_n.jpg?oh=4bdcf7c2b1f815c6dccfc8dde4fcb8ce&oe=5AF1AE07
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26231253_504131910001771_230015825827274796_n.jpg?oh=680e29680a3bc4eb778512e63367dceb&oe=5AB24021
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26219722_504131573335138_8316021168441843615_n.jpg?oh=0038a775603edc5c18e29cb7dd9deeef&oe=5AE4804D
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26229425_504131570001805_6889638982476234911_n.jpg?oh=4e7842e378ae6b9a5fa76be1908666c3&oe=5AEFD505
https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/26229981_504131760001786_1342050876853854206_n.jpg?oh=1814373871b2148226a452b269c22bef&oe=5AF67AAD
