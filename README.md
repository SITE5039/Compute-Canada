# Table of Contents  
[What is Compute Canada?](#header1)  
[Why do you care?](#header2)  
[Creating your Compute Canada account](#header3)  
[Setting up ssh and sftp](#ssh_sftp)  
- [On Windows](#ssh_windows)  
  * [ssh setup on Windows](#header4)  
  * [sftp mount on Windows](#header5)  
- [On Mac/Linux](#ssh_linux)
  * [ssh setup on Mac/Linux](#ssh_setup_linux)  
  * [sftp mount on Mac/Linux](#sftp_setup_linux)  

[Modules and Hello World Program](#header6)  
[Job Launching](#header7)  
[Linux Tutorial](#header8)  
[Globus File Transfer](#header9)  
[**Access to Mist (New!)**](#header10)  
[Some useful commands](#header11)  
  
<a name="header1"/>

# What is Compute Canada?

Please find the pdf of the recent presentation to prof Mao's group 
[here](https://github.com/SITE5039/Compute-Canada/blob/master/working_on_compute_canada.pdf).

Compute Canada is an infrastructure of "supercomputers": four clusters with lots of CPUs/GPUs designed for high performance computing each hosted by a different Canadian university.

The names of the clusters are:
* Arbutus: hosted by U of Victoria
* Cedar: hosted by SFU
* Graham: hosted by Waterloo
* Niagara: hosted by U of T

![](https://github.com/SITE5039/Compute-Canada/blob/master/Capture2.PNG)

Even though they are physically in different parts of the country, all of the clusters are accessible to students/faculty in any Canadian university. So the map above is just to satiate your curiosity -- we can treat all the clusters like they are down the street.

The two clusters called Cedar and Graham are of particular interest because they host a large number of GPUs.

Below are the specs for the Cedar and Graham clusters

![](https://github.com/SITE5039/Compute-Canada/blob/master/Capture1.PNG)

[Here](https://docs.computecanada.ca/mediawiki/images/8/83/General_info.pdf) and [here](https://www.computecanada.ca/wp-content/uploads/2015/02/161125-Tech_Brief_PROOF_2016_EN_05.pdf) you can find general information about the Compute Canada clusters/infrastructure.

The [Compute Canada wiki](https://docs.computecanada.ca/wiki/Compute_Canada_Documentation) is an excellent/comprehensive source of information about Compute Canada. It includes video tutorials and much, much more -- I highly encourage you to have a look.

<a name="header2"/>

# Why do you care?
My understanding is that most (all?) of you are doing thesis work related to machine learning. Most machine learning algorithms require a heavy amount of RAM and CPU resources, and are highly parallelizable.

This means that machine learning algorithms are prime targets for acceleration via high performance computing resources -- whether via high RAM, many-core, or GPUs.

That's just a fancy phrasing to say that your code will run better on a better machine -- duh.

Sometimes that can mean the difference between waiting 10 days for your job to finish versus waiting 1 hour. In other words, Compute Canada can save you time.

As a side benefit, the tools you will learn may be useful on the job market -- assuming you want to work in industry and/or continue doing machine learning work.

<a name="header3"/>

# Creating your Compute Canada account
To get access to the Compute Canada clusters you need to get a Compute Canada account.

This is easy but the process of confirmation takes a few days.

Fill out the application form [here](https://ccdb.computecanada.ca/account_application).

Most fields are obvious, but a few are not:

* On the first page, "Consent to Access User Data"
  * They explain that it's only for support purposes and occasionally needed to help you. I recommend saying yes, because otherwise they may not be able to get into you account to fix things if you need help. Of course this is your choice.
* On the next page, under "Institution", pick "CAC: University of Ottawa"
* For Department, I chose "School of Electrical Engineering and Computer Science"
* For sponsor, type in hkq-940-01
  * This is prof Mao's "Compute Canada Role ID"

You will then get a confirmation e-mail, followed by a verification of your account by Compute Canada, and finally prof Mao will get a confirmation e-mail. The whole confirmation takes a few days.

<a name="ssh_sftp"/>

# Setting up ssh and sftp

<a name="ssh_windows"/>

## On Windows

<a name="header4"/>

### ssh setup on Windows

This is a condensed version of the [Compute Canada ssh instructions](https://docs.computecanada.ca/wiki/SSH). Their instructions, however, cover more options: alternatives to Putty, and setup in Mac/Linux. They also explain why you are doing each step.

#### Test Run
1. [download putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) 
2.  open putty
3.  under Host Name, type \<username\>@graham.computecanada.ca (Note: your username is not your CCI, like abc-123, nor a CCRI like abc-123-01, nor your email address)
4.  Click Open
5.  The first time, it will probably ask you if you trust the connection. Click yes
6.  A terminal should open and it should prompt you for a password. This is your Compute Canada password you used to set up your account.

You're on Compute Canada!

<a name="longer_is_better"/>

#### Complete instructions (passwordless setup)
You will need to follow this longer set of instructions if you want to mount your compute canada drive onto a [Windows](#header5) or [Linux/Mac](#sftp_setup_linux) drive (highly recommended).

These steps will also allow you to avoid typing \<username\>@graham.computecanada.ca every time you log in, and avoid having to type in your password.

##### First time
1. [download putty and puttygen](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) (puttygen comes with putty package)
2. open puttygen
3. Click the "Generate" button. You will then be asked to move your mouse around to generate random data to be used to create the key.
4. Save the "ssh-rsa blablabla" string (public key) that shows up on screen for use in later step
5. Click the "Save private key" button and choose a meaningful file name (e.g. compute_canada.ppk).
6. Click Ok (or X button, I forget) to get out of puttygen
7. open putty
8.  go to Connection->SSH->Auth and clicking the "Browse" button to find the private key file you saved in step 5 
9. under Host Name, type \<username\>@graham.computecanada.ca
10. go back to Session->Saved Session and type graham. Then click save
11. Click Open
12. The first time, it will probably ask you if you trust the connection. Click yes
13. A terminal should open and it should prompt you for a password. This is your Compute Canada password you used to set up your account.
14. type this command, and if it asks you for a passphrase just press enter  
`ssh-keygen -b 2048 -t rsa`
15.  type these command but replace "ssh-rsa blablabla" with the string you saved in step 4:  
`echo "" >> ~/.ssh/authorized_keys`  
`echo "ssh-rsa blablabla" >>  ~/.ssh/authorized_keys`  
16. Now, close your putty terminal. 

##### Every subsequent time
17. Re-open putty.
18. Under Saved Sessions click on Graham
19. click on Load
20. Click Open

You should now be on the Graham server and it should not have asked you for a password:

![](https://github.com/SITE5039/Compute-Canada/blob/master/putty_capture.PNG)

<a name="header5"/>

### sftp mount on Windows

<!---No link to Compute Canada because it is not an official recommendation. --->This will allow you to mount your compute canada drive on Windows. In other words, you will be able to see and edit your files as if they were on your computer. The alternatives to editing files on compute canada are messy.  
<br/>
<br/>
<!---The alternatives are X-forwarding (transfer the screen over the network, very slow), using a Linux shell-editor (very difficult unless you're experienced), or simply copying the files back and forth using sftp or globus (a workaround not a solution). --->

1. Make sure to follow the [passwordless setup](#longer_is_better) instructions. Passwordless ssh is a prereq for the following instructions to work.
2. Open puttygen
3. Click Load and select your private key (eg computecanada.ppk)
4. Under Conversion select Export OpenSSH key.
5. Save your OpenSSH private key with a meaningful name (eg computecanada_rsa)
6. [Download](https://www.nsoftware.com/netdrive/sftp/download.aspx) SFTP Net Drive.
7. Open SFTP Net Drive
8. Create a profile called Graham with hostname graham.computecanada.ca
9. Under username add your compute canada username
10. Under Key browse to the OpenSSH private key you saved in step 5
11. Pick a free drive you wish to mount to (eg typically D: is unused)
12. Click Connect

Now you should be able to navigate to the D: (or whichever you picked) drive on Windows and see all your Compute Canada files. Open text files with the editor of your choice!

<a name="ssh_linux"/>

## On Mac/Linux

<a name="ssh_setup_linux"/>

### ssh setup on Mac/Linux

This is a condensed version of the [Compute Canada ssh instructions](https://docs.computecanada.ca/wiki/SSH). Their instructions, however, cover more options. They also explain why you are doing each step.

#### First time
1. Open Terminal. If you do not already have a ~/.ssh directory, run this command (just press enter if it asks to create a passphrase)  
`ssh-keygen -b 2048 -t rsa`  
2. Copy the contents of your ~/.ssh/id_rsa.pub file ("ssh-rsa blablabla")
3. ssh to graham  
`ssh <username>@graham.computecanada.ca`
4. When prompted enter the Compute Canada password you used to set up your account.
5. type this command (yes, same one as step 1, but now on the remote server), and if it asks you for a passphrase just press enter  
`ssh-keygen -b 2048 -t rsa`  
6. type these command but replace "ssh-rsa blablabla" with the string you saved in step 2:  
`echo "" >> ~/.ssh/authorized_keys`  
`echo "ssh-rsa blablabla" >>  ~/.ssh/authorized_keys`
7. exit out of your ssh session (type exit)

#### Every subsequent time
7. ssh to graham  
`ssh <username>@graham.computecanada.ca`

You should now be on the Graham server and it should not have asked you for a password:

![](https://github.com/SITE5039/Compute-Canada/blob/master/Capture.PNG)

<a name="sftp_setup_linux"/>

### sftp mount on Mac/Linux

#### First time
1. Make sure to follow the [ssh setup](#ssh_setup_linux) instructions. Passwordless ssh is a prereq for the following instructions to work.
2. install sshfs (you'll have to google it -- different steps depending on your OS)  
3. Create the mount directory on your laptop:  
`mkdir <mount point>`  
where \<mount point\> is any directory you choose for mounting the drive. A standard choice in linux would be /mnt/graham  
In Mac the /mnt directory requires some workaround to be usable so a good alternative might be /usr/local/share/graham or \<your desktop\>/graham.

#### Every subsequent time (perhaps added to a script to make life easier)
Run this command to mount:  
`sshfs -o allow_other -o defer_permissions -o follow_symlinks -o ssh_command="ssh -i ~/.ssh/id_rsa" <username>@graham.computecanada.ca:/home/<username> <mount point>`  

<a name="header6"/>

# Modules and Hello World Program
To see what programs are available on Compute Canada, type  
`module avail`  

If you need to use any available module, type  
`module load <module name>`  

To see what modules you have loaded, type  
`module list`  

For a much more detailed discussion of modules visit [Compute Canada's Using Modules wiki](https://docs.computecanada.ca/wiki/Utiliser_des_modules/en  )  

For our Hello World program we will use python 3.6.  

First we run module avail to see if python 3.6 is available. In the output we see  
   python/3.6.3              (L,t,3:3.6)  

So we run  
`module load python/3.6.3`  

Next we create our Hello World program and run it:  
1. Via your windows mounted drive go to your project dir (/home/\<username\>/projects/def-yymao/\<username\>)
2. Create a file with this content  
#!/usr/bin/env python3.6  
print("Hello World")  

3. Name this file test.py
4. Via putty cd to your project dir  
`cd /home/<username>/projects/def-yymao/<username>`
5. Make your file executable  
`chmod +x test.py`
6. Run your file  
`./test.py`  

This should be printed to the putty console window:  
Hello World

<a name="header7"/>

# Job Launching

Quick test:
1. cd to your project dir  
`cd /home/<username>/projects/def-yymao/<username>`
2. Copy the test SLURM launch script to your project dir  
`cp /home/<username>/projects/def-yymao/common/launch_sim.sh .`
3. Run it  
`sbatch launch_sim.sh`  

This should print the following output to a file called \<job id\>.out:  
Hello World  

For all the detail on launching jobs and other useful information about Graham, I highly recommend this [Compute Canada training video](https://www.youtube.com/watch?v=IiAbxPZ3BHo).    

The [Compute Canada wiki on running jobs](https://docs.computecanada.ca/wiki/Running_jobs) is also excellent.

<a name="header8"/>

# Linux Tutorial

[Compute Canada's wiki on using Linux](https://docs.computecanada.ca/wiki/Linux_introduction) is an excellent starting point and will give you immediate working knowledge.

For a larger set of commands still geared for a beginner I highly recommend [this tutorial](http://www.ee.surrey.ac.uk/Teaching/Unix/).

My takeaway from the latter tutorial:  
<br/>
1 to 3. Critical. These sections cover all the commands I heavily use in Linux, and it looks really well explained. It's really worth spending the time to read carefully and understand.  
<br/>
4. Also critical, except the "whatis" and "apropos" commands, which are still useful but you can live without. The rest of the commands are just as important (or almost) as those in sections 1-3.  
<br/>
5. Very relevant, but a shade more advanced. You can live without these commands for a while, but eventually you will need them. I would read diagonally and come back to it later when you find a need for these.  
<br/>
6. Read the "find" and "history" commands, which are very important. The rest are more advanced commands I hardly ever use. "find" is actually one of the most important commands of all, and history is important too. I'm not sure why they bunched these two pearls with more obscure commands. Spend the time to read "find" and "history" but skip the rest.  
<br/>
7. More advanced stuff. Skip it

<a name="header9"/>

# Globus File Transfer

Globus is an important tool for transferring large files. If you find yourself in need of making large file transfers I highly recommend it as it provides interruption recovery and e-mails you when the transfer is done.

The [Compute Canada wiki on Globus](https://docs.computecanada.ca/wiki/Globus) explains how to set it up and use it.

Caveat: Globus does not currently work over the eduroam wireless network (due to the firewall). You need to either use it on your home (or other) network, or be plugged in on campus via ethernet. Alternatively, you could use a VPN on campus.

<a name="header10"/>

# Access to Mist (updated on 2020/9/24)

[Mist](https://docs.scinet.utoronto.ca/index.php/Mist) is also a Compute Canada computation resource (it’s a part of the [Niagara](https://docs.scinet.utoronto.ca/index.php/Niagara_Quickstart) supercomputer that is located at University of Toronto). If you already have a Compute Canada account, please follow the steps shown below:

1. Go to the [Compute Canada Database (CCDB) site](https://ccdb.computecanada.ca/security/login) and login ---> Go to “My Account” and “Request access to other clusters” ---> Click “Request access to Niagara and Mist” and “Join”.
2. Send an email with your CCDB username and SOSCIP project ID (3-056) to soscip-support@scinet.utoronto.ca and keep Prof. Mao (ymao@uottawa.ca) cc-ed on that email. Your email subject could be “Request my account to be added to the SOSCIP allocation”. They will notify you by responding to the email once you have been added to the SOSCIP allocation. 
3. Use the command `ssh -Y username@mist.scinet.utoronto.ca` to access Mist directly.


Basically, the way of using Mist is almost the same as our familiar GPU clusters, viz. Cedar, Graham and Beluga. Here are some main differences:
1. SOSCIP users must submit jobs using their SOSCIP project allocation. To do so, you need to add this line in your job scripts:  
`#SBATCH --account=soscip-3-056`  
Notice that on Cedar/Graham/Beluga, we use `#SBATCH --account=def-yymao` or `#SBATCH --account=rrg-yymao`.
2. Your job's maximum running time is 24 hours.
3. **The $HOME directory is read-only on the compute nodes. You should not submit from your $HOME directory, but rather, from your $SCRATCH directory, so that the output of your compute job can be written out. Please take a look at https://docs.scinet.utoronto.ca/index.php/Data_Management .**  
If you want to login to the $SCRATCH directory directly every time, you can find your .bash_profile file in your home directory:  
`[root@nia-login03 ~]# ls -l ~username/.bash_profile`  
and add "cd $SCRATCH" in this file (via nano or other commands).
Although you cannot sumbit jobs from your $HOME directory, your Conda environment could live in your $HOME directory.  
4. The easiest way to install PyTorch on Mist is using IBM's Conda channel, as mentioned in [Mist wiki page](https://docs.scinet.utoronto.ca/index.php/Mist).    
    ```
    module load anaconda3
    conda create -n pytorch_env_name python=3.7
    source activate pytorch_env_name
    conda install -c https://public.dhe.ibm.com/ibmdl/export/pub/software/server/ibm-ai/conda-early-access/ pytorch 
    ```  
    Notice that instead of the IBM's Conda channel in [Mist wiki page](https://docs.scinet.utoronto.ca/index.php/Mist), we use the IBM early-access Conda channel here. Once the installation finishes, please remember to clean the cache:    
    ```
    conda clean -y --all
    rm -rf $HOME/.conda/pkgs/*
    ```
    An example script for a pytorch job on Mist:
    ```
    #!/bin/bash
    #SBATCH --nodes=1
    #SBATCH --gpus-per-node=1
    #SBATCH --time=10:59:59
    #SBATCH --account=soscip-3-056 #For SOSCIP projects only
    #SBATCH --ntasks=32
    #SBATCH --output=filename-%N-%j.out
    #SBATCH --mail-user=XXXXX@XXX.com
    #SBATCH --mail-type=ALL
    module load anaconda3
    source activate pytorch_env_name
    python main.py --model ResNet18 --method MG --epoch 500 --lr 0.0008 --batch_size 16
    ```

More comments:
* /scratch is to be used primarily for temporary or transient files, checkpoint dumps, for all the results of your computations and simulations. Moreover, you may lose your files in /scratch which have not been accessed for more than TWO months. One option to keep your datasets is to put them in /home, for example, you could download the CIFAR-10 dataset in $HOME/datasets, and when you use pytorch in /scratch, you can write something like:
    ```
    from torchvision import datasets, transforms
    dataset = datasets.CIFAR10(root='/home/y/yymao/geothe9/datasets', download=False, train=True, transform=transform)
    ```
    Replace 'geothe9' with your own username.

<a name="header11"/>

# Some useful commands
* Check all the current job status of our group's by
    ```
    squeue -A def-yymao_gpu,def-yymao_cpu                                # For Graham and Beluga
    squeue -A def-yymao_gpu,def-yymao_cpu,rrg-yymao_gpu,rrg-yymao_cpu    # For Cedar
    squeue -A def-yymao_gpu,def-yymao_cpu,soscip-3-056                   # For Mist
    ```
    For example, the status "PD" means your job is waiting and "R" means your job is running.
* Every group has a target usage level. Check the (GPU) job priority and fair-share by:
    ```
    sshare -l -A def-yymao_gpu          # For Graham, Beluga and Cedar
    sshare -l -A rrg-yymao_gpu          # For Cedar
    sshare -l -A soscip-3-056 -a        # For Mist
    ```
    In short, high "LevelFS" indicates high priority. 
