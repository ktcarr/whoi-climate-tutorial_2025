# Accessing the CMIP data servers

## CMIP6
In the tutorials, we'll use data from the 6th version of the Coupled Model Intercomparison Project, or "CMIP6". While this data is publicly available for download, WHOI has downloaded lots of it to a shared data server (technically, a "network file share" or NFS). Now, we'll connect to this WHOI server so we don't have to download the data. Once we're connected, we'll be able to manipulate the data on the server as if it was downloaded locally. The following instructions are reproduced from [WHOI's CMIP6 data access page](http://cmip6.whoi.edu/?page_id=50).

```{note}
To connect to the server, you must be on the WHOI network (i.e., on the WHOI wifi or connected by VPN).
```

### Connecting from a Mac PC
Open Finder, then select "Go" from the top menu bar and click "Connect to Server" (at the very bottom of the dropdown). Then, enter ```smb://vast.whoi.edu/proj/cmip6```. If prompted, enter your WHOI username (*without* "@whoi.edu") and password. Note the default mount location for the server is ```/Volumes/data```.


### Connecting from a Windows PC
In the file explorer, navigate to “This PC” and select **Computer > Map network drive**.

```{figure} figs/nfs-win-01.png
---
width: 500px
name: nsfwin1
---
```

In the dialog enter ```\\vast.whoi.edu\proj\cmip6``` for **Folder**, and check the box for **Connect using different credentials**.

```{figure} figs/nfs-win-02.png
---
width: 500px
name: nsfwin2
---
```

Enter your **WHOI username** (with domain, e.g. “@whoi.edu”) and **password**. Check the box for **Remember my credentials**.

```{figure} figs/nfs-win-03.png
---
width: 500px
name: nsfwin3
---
```

### Connecting from a (non-Mac) LINUX PC
The following packages are required for mounting the NFS on Linux devices.
- ```cifs-utils```: Common Internet File System utilities
- ```samba-common```: common files used by both the Samba server and client
- ```samba-common-bin```: common files used by both the Samba server and client
To install these packages on Debian Linux and related variants, for example, use:
```bash
sudo apt-get install cifs-utils samba-common samba-common-bin
```

To mount the NFS, use the following (replace ```USERNAME``` and ```PASSWORD``` with your own credentials):
```
sudo mkdir /mnt/cmip6-data
sudo mount -t cifs -ouser=USERNAME,password=PASSWORD //vast.whoi.edu/proj/cmip6 /mnt/cmip6-data
```

### Connecting from Poseidon
The CMIP6 server is accessible on Poseidon at ```/vortexfs1/share/cmip6```.

## CMIP5
For CMIP5 (instead of CMIP6), use the same process but replace ```vast.whoi.edu/proj/cmip6``` with ```cmip5.whoi.edu```.
