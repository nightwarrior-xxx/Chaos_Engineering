How to recover deleted “dpkg” directory?




Unfortunately I've deleted dpkg directory while removing the lock. By mistake I typed

root@sam:~$ rm -r /var/lib/dpkg

Now when I am trying to install/uninstall packages it shows me following error.

E: Could not open lock file /var/lib/dpkg/lock - open (2: No such file or directory)






<pre>ls -l /var/lib/dpkg/</pre>
total 9964
drwxr-xr-x 2 root root    4096 nov 28 11:18 alternatives
-rw-r--r-- 1 root root      11 sep 18 14:08 arch
-rw-r--r-- 1 root root 2573807 nov 28 11:18 available
-rw-r--r-- 1 root root 2561322 nov 28 10:25 available-old
-rw-r--r-- 1 root root       8 abr 24  2013 cmethopt
-rw-r--r-- 1 root root     538 sep 25 17:24 diversions
-rw-r--r-- 1 root root     457 sep 25 17:24 diversions-old
drwxr-xr-x 2 root root  483328 nov 28 11:17 info
-rw-r----- 1 root root       0 nov 28 11:18 lock
drwxr-xr-x 2 root root    4096 mar 22  2013 parts
-rw-r--r-- 1 root root     135 abr 24  2013 statoverride
-rw-r--r-- 1 root root 2269113 nov 28 11:18 status
-rw-r--r-- 1 root root 2268870 nov 28 11:18 status-old
drwxr-xr-x 2 root root    4096 nov 28 11:18 triggers
drwxr-xr-x 2 root root    4096 nov 28 11:18 updates

You removed 5 directories, the status file, etc. So, lets try to fix the stuff. First, create the directory:

<pre>sudo mkdir -p /var/lib/dpkg/{alternatives,info,parts,triggers,updates}</pre>

Recover some backups:

<pre>sudo cp /var/backups/dpkg.status.0 /var/lib/dpkg/status</pre>

Now, let's see if your dpkg is working (start praying):

<pre>apt-get download dpkg</pre>
<pre>sudo dpkg -i dpkg*.deb</pre>

If everything is "ok" then repair your base files too:

<pre>apt-get download base-files</pre>
<pre>sudo dpkg -i base-files*.deb</pre>

Now try to update your package list, etc.:

<pre>dpkg --audit</pre>
<pre>sudo apt-get update</pre>
<pre>sudo apt-get check</pre>
