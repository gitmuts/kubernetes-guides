I recently faced this error "no space left on device", on checking the space left on the disk it was more than enough.

The system was maxing out on memory at some point, I suspected it to be a resource issue. We added more RAM, CPU and mounted a new disk.

The article below will be on the actions we did, and maybe find out if it was the right thing to do or not.


inotify issue: https://github.com/tilt-dev/tilt/issues/3916

[inotify monitors changes to files and notifies applications.](https://en.wikipedia.org/wiki/Inotify)

Does "kubectl logs " use inotify?

 ** inodes
 ** changing docker default directory
 ** allocating resources to containers
 
 
Script to check inotify used: https://github.com/fatso83/dotfiles/blob/master/utils/scripts/inotify-consumers


More troubleshooting:
cat /proc/sys/fs/inotify/max_user_watches

If 8k increase

sudo sysctl fs.inotify.max_user_watches=1048576
sudo sysctl -p
