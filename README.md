This is a OpenLane SoC project Repository.

#Setting Up Shared Folder Between Windows and Linux VirtualBox
While working on my RISC-V SoC project in the Linux VirtualBox environment, I needed to share screenshots and files from my Linux VM to my Windows system. Here’s exactly what I did, step by step:

1. Creating a Shared Folder in Windows
First, I created a separate folder in my Windows system to keep things organized and avoid mixing with other screenshots.
I planned to use this folder whenever I wanted to move files between Linux and Windows — not just for OpenLAN, but for other Linux-related tasks too.

2. Configuring Shared Folder in VirtualBox Settings
Then, I configured this folder as a shared folder in VirtualBox:
I completely powered off the virtual machine (not saved state).
Then I opened VirtualBox > Settings > Shared Folders for my VM and added the Folder.

