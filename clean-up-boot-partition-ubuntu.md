
# Safest way to clean up boot partition - Ubuntu 14.04LTS-x64, Ubuntu 16.04LTS-x64

[Reference](http://askubuntu.com/questions/345588/what-is-the-safest-way-to-clean-up-boot-partition)

## Case I: if /boot is not 100% full and apt is working

### 1. Check the current kernel version
```
$ uname -r 
```

It will shows the list like below:
```
3.19.0-64-generic
```

### 2. Remove the OLD kernels

#### 2.a. List the old kernel

```
$ sudo dpkg --list 'linux-image*'|awk '{ if ($1=="ii") print $2}'|grep -v `uname -r`
```

You will get the list of images something like below:

```
linux-image-3.19.0-25-generic
linux-image-3.19.0-56-generic
linux-image-3.19.0-58-generic
linux-image-3.19.0-59-generic
linux-image-3.19.0-61-generic
linux-image-3.19.0-65-generic
linux-image-extra-3.19.0-25-generic
linux-image-extra-3.19.0-56-generic
linux-image-extra-3.19.0-58-generic
linux-image-extra-3.19.0-59-generic
linux-image-extra-3.19.0-61-generic

```

#### 2.b. Now its time to remove old kernel one by one as 

```
$ sudo apt-get purge linux-image-3.19.0-25-generic
$ sudo apt-get purge linux-image-3.19.0-56-generic
$ sudo apt-get purge linux-image-3.19.0-58-generic
$ sudo apt-get purge linux-image-3.19.0-59-generic
$ sudo apt-get purge linux-image-3.19.0-61-generic
$ sudo apt-get purge linux-image-3.19.0-65-generic
```

When you're done removing the older kernels, you can run this to remove ever packages you won't need anymore:

```
$ sudo apt-get autoremove
```

And finally you can run this to update grub kernel list:

```
$ sudo update-grub
```



## Case II: Can't Use `apt` i.e. /boot is 100% full

<strong style="color:red">NOTE: this is only if you can't use apt to clean up due to a 100% full /boot</strong>


### 1. Get the list of kernel images
Get the list of kernel images and determine what you can do without. This command will show installed kernels except the currently running one

```
$ sudo dpkg --list 'linux-image*'|awk '{ if ($1=="ii") print $2}'|grep -v `uname -r`
```

You will get the list of images somethign like below:

```
linux-image-3.19.0-25-generic
linux-image-3.19.0-56-generic
linux-image-3.19.0-58-generic
linux-image-3.19.0-59-generic
linux-image-3.19.0-61-generic
linux-image-3.19.0-65-generic
linux-image-extra-3.19.0-25-generic
linux-image-extra-3.19.0-56-generic
linux-image-extra-3.19.0-58-generic
linux-image-extra-3.19.0-59-generic
linux-image-extra-3.19.0-61-generic

```

### 2. Prepare Delete
 Craft a command to delete all files in /boot for kernels that don't matter to you using brace expansion to keep you sane. Remember to exclude the current and two newest kernel images. 
From above Example, it's 
```
sudo rm -rf /boot/*-3.19.0-{25,56,58,59,61,65}-*
```

### 3. Clean up what's making apt grumpy about a partial install.
```
sudo apt-get -f install
```

### 4. Autoremove
Finally, autoremove to clear out the old kernel image packages that have been orphaned by the manual boot clean.

```
sudo apt-get autoremove
```

### 5. Update Grub
```
sudo update-grub
```

### 6. Now you can update, install packages
```
sudo apt-get update
```