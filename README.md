# Configuring ISCA Guide

This guide is intended for new PhD students in the Geoff Vallis group, although many components of this guide will be helpful for anyone hoping to set up ISCA. It also assumes the user is using a Unix (Apple/Linux) based machine. Please contact somebody else if you're a windows user as additional steps for setting up PUTTY are required before proceeding with the guide. This is also meant to be illustrative rather than exhaustive.

**This guide was heavily inspired (and the majority was in fact copied directly) from Spencer Clark at GFDL, so he deserves all the credit.**

Last Edited 24/09/19 by Brett Mckim (brettmckim@gmail.com). I welcome questions and comments.

## Account Access

The `gv` (Geoff Vallis) machines are what most people in the group use to run ISCA. Larger simulations may require access to the Exeter supercomputer, but we will not focus on that here. To gain access to the 5 `gv` machines contact Robert O' Neale (r.oneale@exeter.ac.uk).

Ideally, computations are distributed evenly across the 5 machines, so ask Geoff which one to start out with.

If you want to be able to work from home, install `pulsesecure` from Exeter University as it gives you a university VPN.

## Connecting to the `gv` machines

As a student you should have a single sign on username and password. Use these to login to the gv machine you've been approved to use. Let's use `bam218` and `gv5` as our example, but please use your machine and account.

Note that throughout this document I denote lines to be typed in a shell by starting them with a dollar sign ( `$` ); the dollar sign is not to be typed as part of the command.

## Basic Command-Line SSH

Let's first show logging in without any tricks. In the shell type the following command, and then enter your single sign on password.

`$ ssh bam218@emps-gv5`

This logs you into gv5. Congrats! The first thing you should do is create your own directory. Do this by going to scratch and making a directory with your account name.

```
$ cd /scratch
$ mkdir bam218
```

Type `ls` and you should see your account along with everyone else.

## Creating a basic SSH config file

If you use a Unix-based operating system on your personal computer, you can make use of an SSH config file to create shortcuts to your frequently used remote computers. I will start simple then progressively add settings explaining what they enable.

In the home directory of your personal computer, create a new directory called `.ssh` if it doesn't exist already. Navigate to this directory, and create a file called `config` using your favorite text editor (I use emacs because its also available on the gv machines). Put in the following contents.

```
Host gv
	Hostname emps-gv5
	User bam218
```
We have now setup a shortcut to log in to gv5 from the command line. In a new terminal window type:

`$ ssh gv`

And then enter your password.

## What can we do to simplify logging in to `gv`?

A lot actually, but we'll take it one step at a time. Our first step will require a foray into port forwarding. This is not the first time we will encounter port forwarding in this talk, so I'll take the opportunity to introduce it. Per the Wikipedia article for Port Forwarding, local port forwarding allows a user on a local machine to create a secure tunnel to a remote server:

> The Secure Shell client is configured to redirect data from a specified local port through the secure tunnel to a specified destination host and port.

Creating an SSH tunnel is useful because it allows a user to set up a secure path to a server that is behind a firewall (like gv). Let's set up an SSH tunnel that maps `localhost` port 3039 on my local machine to port 3039 (the number is arbitrary as long as its 1024 and less than 49150) on the address `emps-gv5`

`$ ssh -l localhost:3039:emps-gv5:3039 bam218@emps-gv5`

Essentially what this means that whenever we try to access `localhost:3039` on our local machine, we are actually accessing emps-gv5:3039 from behind the firewall. Let's add this to our SSH config file so that this tunnel always exists. This requires adding a line under the `gv` host.

```
Host gv
	Hostname emps-gv5
	User bam218
	LocalForward 3039 localhost:3039
```
By using an SSH config file, secure methods for copying (e.g. `scp` or `sftp` can now use the same host aliases. Let's say I want to copy a file from my home directory on `gv5` using `scp` into my current working directory on my local machine. This is now as simple as:

`$ scp localfile.txt gv:/scratch/bam218/`

## Can we simplify logins to `gv` further?

The answer is yes. If you have a Mac, we can eliminate the need to enter a password every time we want to log in by using an SSH key pair.

To do this, go to your local computer and enter the following command to generate a set of RSA keys:

`$ ssh-keygen -t rsa`

You will then be prompted to supply a filename and a password. For the file name I recommend `id_rsa_gv` and the password to be the same as your single sign on with Exeter. It is good practice to save these keys to your `~/.ssh` directory. If you use a Mac, the operating system can use its internal keychain to remember your password, therefore you won't need to type this password in every time you log in!

This will generate two files: one is the so-called "private key" (the one without the `.pub` extension), and the other is the so-called "public key". You keep your private key strictly on your local computer. You need to copy your public key to the remove machine you would like to use the key pair to log in to on. This can be done through `scp`:

`$ scp ~/.ssh/id_rsa_gv.pub gv:/scracth/bam218/.ssh/`

Note: you need to create the `.ssh` directory on your `gv` machine to do this.

After copying the public key over to `gv`, one now needs to append to or create an `authorized_keys` file in their `~/.ssh/` directory. 'Copy the contents of the public key you just copied over into this `authorized_keys` file.

Now from a terminal window on your local machine, you can try logging in to `gv` with the following command:

`$ ssh -i ~/.ssh/id_rsa_gv gv`

This will prompt you for the password you specified upon creating your key pair using `ssh-keygen` . To always make use of your private key when logging in to `gv`, modify the `gv host` in your `config` file to add an `IdentityFile` to your setup as well as `UseKeychain yes` and `AddKeysToAgent yes`. Now you should no longer need to enter your password again!

Your final `config` file should look like this:

```
Hostname emps-gv5
	User bam218
	LocalForward 3039 localhost:3039
	IdentityFile ~/.ssh/id_rsa_gv
	UseKeychain yes
	AddKeysToAgent yes
```

## Setting Up Anaconda

Anaconda is a convenient package manager for installing python and related modules. Set it up now and it will make installing packages necessary for ISCA a lot easier.

Navigate to the Anaconda website, and locate the download tab. Select the linux option and right click on the download button to copy the link. In your `gv` machine, type the following command in your home directory:

`wget anaconda_link.html`

Making sure to replace the link above with the one you copied. In the directory you downloaded Anaconda, run the installer with:

`bash Anaconda_...`

Just tab complete the command to get the name of the installer. Select yes for all the agreements/options they show you. Installing should take anywhere from 5 - 30 minutes. Once you are done, you are ready to install ISCA!

## Downloading ISCA

Travel to the following link: https://github.com/ExeClim/Isca and follow the sections titled "Installing the `isca` pthon module" and "Compiling for the first time" exactly. Download these files to your `\scratch\bam218\` directory. This is ISCA.

Now travel to https://github.com/ExeClim/ictp-isca-workshop-2018 and clone the repository to your `\scratch\bam218\` directory.  This includes some helpful tools to analyze the first ISCA simulation you are soon to set up.

## Running Python from Jupyter Notebooks

I find [Jupyter notebooks](https://jupyter.org) to be a fantastic environment for prototyping code, making figures and general data exploration. That being said, Jupyter notebooks rely on a browser for their use. To open your first Jupyter notebook from `gv` type: 

`$ jupyter lab --no-browser --port=3039`

This should function because of all the work we put in during the port forwarding section. To shorten this command, add an alias to your .bashrc file, which should be located in your home directory. I use the alias  `rjlab`.

## Running ISCA

Here is where the fun begins. ISCA can be configured in many ways, but the simplest setup is the Held-Suarez Configuration. Ask your peers to learn more about what the Held-Suarez configuration *is*. Navigate to your `/scratch/bam218/Isca/exp/test_cases/held_suarez` directory and from there, type:

`$ python held_suarez_test_case.py`

Since this is your first time running ISCA, the fortran source code needs to be compiled which should take a while. Once this is done the model will run for 12 months and you should be able to see the simulation progress on the command line. This should work without any hiccups.

## Post Processing

Your first simulation should have outputted to a directory found in: `/scratch/bam218/gfdl_data/held_suarez_default`. Take a look around to see how files are organized. Now we can look at the data by copying some analysis files used in a previous ISCA workshop.

I chose to copy the contents of `/scratch/bam218/ictp-isca-workshop-2018/analysis/analyse_single_experiment.py` into a new notebook (which end with the abbreviation `ipynb`.  To get my code to work, I had to add 3 commands:

```
import sys
sys.path.insert(1, '/scratch/bam218/ictp-isca-workshop-2018/analysis')
%matplotlib inline
```
 
 The first 2 lines are to tell the notebook where to look to find Stephen's other useful scripts. The third line is simply a magic command which is specific to notebooks (as opposed to .py files). This one allows us to show our plots within the notebook.

When you try to run the script, you might find you need to download a couple more python modules. With anaconda this is quite easy. If I need to download `netcdf4` for example, I'll just google "conda install netcdf" and I'll find a link which gives me the exact code. In this case it is `conda install -c anaconda netcdf4`. Simple, really.

The analysis file assumes the run lasts 24 months, but ours only lasts 12, so change that.

The only other thing to note is the conda environment you are in. Anaconda makes it easy to keep python distributions separate from one another in case you have modules which have conflicting requirements for use. (A much better explanation can be found [here](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=2ahUKEwjjv_31vOvkAhViTBUIHamnAR4QtwIwAHoECAUQAQ&url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DN5vscPTWKOk&usg=AOvVaw3FbQrIrQIzywlnpwvawmrR). Just make sure you are in the environment called `isca_env`. If not you just type `conda activate isca_env`.

The final result of all the steps above should be a python notebook which looks something like this:

```python
import sys
sys.path.insert(1, '/scratch/bam218/ictp-isca-workshop-2018/analysis')
import xarray as xar
import os
import matplotlib.pyplot as plt
import analyse_functions as af
%matplotlib inline
```

```python
#Folder name where data lies. Script will assume this data is in $GFDL_DATA directory
exp_folder_name = 'held_suarez_default'
#exp_folder_name = 'frierson_grey_rad_analysis_example'

#First month to be used
start_file = 1
#Final month to be used
end_file = 12

#Name of individual netcdf files to include. Example for monthly data.
file_name = 'atmos_monthly.nc'

#Open netcdf files as an xarray dataset object
dataset = af.open_experiment(exp_folder_name, start_file, end_file, file_name)

#Time and zonally average zonal wind and make contour plot
dataset.ucomp.mean(('lon','time')).plot.contourf()
plt.ylim(dataset.pfull.max(), 0.)
plt.title('Time-averaged zonal-mean zonal wind')

#Perform area mean of atmospheric temperatures and make contour plot
af.global_average_lat_lon(dataset, 'temp')
plt.figure()
dataset.temp_area_av.transpose('pfull', 'time').plot.contourf()
plt.ylim(dataset.pfull.max(), 0.)
plt.title('Time and height evolution of area-mean atmospheric temperature')

#Monthly-mean temp at 250hPa
dataset.temp.sel(pfull=250.,method='nearest').groupby('months').mean('time').plot.contourf(col='months', col_wrap=4)

#Seasonal mean zonal-mean zonal-wind
dataset.ucomp.groupby('seasons').mean(('lon','time')).plot.contourf(col='seasons', col_wrap=2)
plt.ylim(dataset.pfull.max(), 0.)

plt.show()
```


![enter image description here](https://lh3.googleusercontent.com/OBk9o51zvy1ErphpLT7hhL6zwR621TvIRPGq9CrNGIOGtogVwPnHvXmdPeZQ-_MDUkJsDMj9JSXXdQ)


![enter image description here](https://lh3.googleusercontent.com/lcdWC-dQXyELy4k6Yt8cSB_MPFlC5YaAJFZ5ZqCLb1I4JfGDWGmxsAvLU5KkFdtGEbdAMBUzR3_5tw)


![enter image description here](https://lh3.googleusercontent.com/nQL4UqEWadC4VIt729q4xPckVCjsrGSwGsNgaCMaH5cOmb0NAx-jLgLEhJ-X9h6Q8mvpaB3Pmp2nrg)


![enter image description here](https://lh3.googleusercontent.com/hZ8gPG749nvWQB-sunlCZ3MxTxAqcT3mMAznQh8g3P-R5Qpcu4imHRDr_-o4b28KgOota5wvMAaY7w)




# All Done!

