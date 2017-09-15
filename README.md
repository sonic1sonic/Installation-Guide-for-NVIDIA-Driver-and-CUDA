# Install NVIDIA Driver and CUDA on Ubuntu 16.04 with GUI Running on Integrated On-board Graphics

## Prerequisites

*   Bootable USB Drive of Ubuntu **Server** 16.04 LTS

*   Download your NVIDIA Driver from [here](http://www.nvidia.com/Download/index.aspx) and put it in the bootable USB (don't worry, it's fine)

*   Download your CUDA Toolkit from [here](https://developer.nvidia.com/cuda-downloads) and put it in the bootable USB

*   I use *NVIDIA-Linux-x86_64-384.69.run* and *cuda_8.0.61_375.26_linux.run* for example in the following steps

## BIOS

*   BIOS &rarr; Boot Priority

    *   Drag the UEFI bootable USB device to the top priority

*   BIOS &rarr; Advanced Mode (F7) &rarr; System Agent (SA) Configuration &rarr; Graphics Configuration &rarr; Primary Display

    *   Primary Display : `IGFX`
    *   DVMT Pre-Allocated : `1024M`

*   Reboot

## Install Ubuntu Server 16.04 LTS

*   Nothing special, the installation should finish in 15 mins

## Copy NVIDIA Driver and CUDA Toolkit

*   Mount the bootable USB again

    *   use `lsblk` to determine which **partition** to mount
    *   `sudo mount /dev/sdx /media/cdrom`

*   Copy the file

    *   `cp /media/cdrom/NVIDIA-Linux-x86_64-384.69.run ~/`
    *   `cp /media/cdrom/cuda_8.0.61_375.26_linux.run ~/`

*   Unmount

    *   `sudo umount /dev/sdx`

## Install NVIDIA Driver

*   `sudo apt-get install build-essential`

*   `sudo sh NVIDIA-Linux-x86_64-384.69.run —no-opengl-files`

    *   **(very important)** `--no-opengl-files` flag will prevent NVIDIA driver from **destroying** you unity desktop (which will be installed later)

*   >Please read the following LICENSE and then select either “ACCEPT” to accept the license and continue with the installation, or select “Do Not Accept” to abort the installation.

    *   `Accept`

*   >WARNING: Nvidia-installer was forced to guess the X library path ‘usr/lib/’ and X module path ‘/usr/lib/xorg/modules’; these paths were not queryable from the system, If X fails to find the NVIDIA X driver module. Please install the ‘pkg-config’ utility and the X.Org SDK/development package for your distribution and reinstall the driver.

    *   `OK`

*   >WARNING: Unable to find a suitable destination to install 32-bit compatibility libraries. Your system may not be set up for 32-bit compatibility. 32-bit compatibility files will not be installed; if you wish to install them, re-run the installation and set a valid directory with the --compat32-libdir option

    *   `OK`

*   >Would you like to run the Nvidia-xconfig utility to automatically update your X configuration file so that the NVIDIA X driver will be used when you restart X? Any pre-existing X configuration will be backed up

    *   `OK`

*   >Installation of the NVIDIA Accelerated Graphics Driver for Linux-x86_64 (version: 384.69) is now complete. Please update your XF86Config or xorg.conf file  as appropriate; see the file /usr/share/doc/NVIDIA_GLX-1.0/README.txt for details 

    *   `OK`

*   After the installation, execute `nvidia-smi` to see if you have made it or not

##  Install CUDA Toolkit

*   `sudo chmod +x cuda_8.0.61_375.26_linux.run`

*   `sudo sh cuda_8.0.61_375.26_linux.run`

*   >Do you accept the previously read EULA?

    *   `accept`

*   >Install NVIDIA Accelerated Graphics Driver for Linix-x86_64 375.26?

    *   `no` (**very important**)

*   >Install the CUDA 8.0 toolkit?

    *   `yes`

*   >Enter Toolkit Location [ default is /usr/local/cuda-8.0]

    *   wherever you want or just simply press <kbd>Enter</kbd>

*   >Do you want to install a symbolic link at /usr/local/cuda?

    *   `yes`

*   >Install the CUDA 8.0 Samples?

    *   `yes` or `no`, suit yourself

*   >Enter CUDA Samples Location [ default is /home/USERNAME ]

    *   wherever you want or just simply press <kbd>Enter</kbd>

## Set the Environment Variables

*   `sudo vim ~/.bashrc`

*   add `export PATH=/usr/local/cuda-8.0/bin:$PATH`

*   add `export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:$LD_LIBRARY_PATH`

*   `:wq`

*   `source .bashrc` to reload .bashrc file

*   execute `nvcc --version`, you should see some information about your CUDA displayed

## Install Desktop Environment (GUI)

*   `sudo apt-get install ubuntu-desktop`

    *   installation may take about 30 mins

*   >W: madam: /etc/mdadm.conf defines no arrays

    * you may see this at the end, it's fine

## Finale

*   `sudo reboot`

*   After rebooting your computer, you should be able to see the desktop after login. However, if you stuck in a [login loop](https://askubuntu.com/questions/762831/ubuntu-16-stuck-in-login-loop-after-installing-nvidia-364-drivers), then you're really screwed up, try some solution on the Internet or just install it again

*   **Congratulations!** if you can access your desktop environment and there's nothing related to GUI (e.g., compiz, xorg) running on your GPU (check it with `nvidia-smi`)
