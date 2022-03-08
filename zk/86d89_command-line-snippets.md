# Command Line Snippets

A collection of small snippets good for learning and building on top of, many from [climagic](https://twitter.com/climagic/).

## Terminal Comment

Using `# comment` in terminal commands makes history searching convenient.

```sh
$ ping 172.217.19.174 # google
back-i-search: google_
```

## Find and Compress Old Files

Find non-empty files over 4 days old under the current directory without a .gz extension and compress them.

```sh
find . -mtime +4 ! -name '*.gz' ! -empty -execdir gzip -v9 {} +
```

## Extract and Pipe from CSV

Take the 3rd and 4th column from a csv file (which contained user/password pair) and pass it directly into chpasswd to set all the passwords for a bunch of accounts.

```sh
cut -d, -f3-4 --output-delimiter=: userlist.csv | chpasswd
```

## Less Tail

The `-F` option or pressing `F` in less is similar to `tail -f filename.log` but can use less's features.

```sh
less -F filename.log
```

## Grep Show Surrounding

Use the -B option of grep to provide 5 (B)efore lines of context so that you can then search that context for something else, like the username to look for commonalities.

```sh
grep -B5 "ldap server connection error" maillog | grep user=
```

Mnemonics for options:
* (B)efore
* (A)fter
* (C)irca/context

## Awk Header and Lines

An awk way to print the first and last line of a file easily using awk so that for example you can see the headers along with the most recent entry for data.

```sh
awk 'NR==1{print} END {print}' data.csv
```

Similarly useful for searching for particular lines in a CSV while including the headers, print the header row and then any lines where the first column contains /pattern/ and the 3rd column is more than 100.

```sh
awk 'NR==1 || ($1 ~ /pattern/ && $3 > 100)'
```

## Tree JSON Output

Easily makes a full filesystem json output including full path, file permissions, owner, size, etc.

```sh
sudo tree -Jhfpug /
```

## Mount Full Disk Image

If you have a full image copy of a disk (floppy, hard drive, thumb drive, CD-ROM iso, etc), you can virtually mount that disk image file under Linux using a loopback driver so you can examine/modify the contents.

```sh
sudo mount -o loop diskimage.img mountpoint
```

If it's a forensic image, remember to add ",ro" to the options.

## Generate Passwords

Use a for loop and pwgen to quickly create a list of 50 users numbered 01 through 50 and a unique random strong password for each one, saving the output to a file named users.txt

```sh
for i in user{01..50} ; do echo "$i $( pwgen 16 1 )" ; done > users.txt
```

Once saved into users.txt. We can use the chpasswd to update password in bulk for those users

```sh
while read -r usr pwd; do echo "${usr}:${pwd}" | chpasswd; done < users.txt
```

if you have a list of existing user names. Don't forget to delete users.txt when you're done to avoid a potential incident or GDPR headache in the future

```sh
for i in $(cat usernames.txt); #
```

Alternative solution:

```sh
echo user{01..50}' '$(pwgen 16 1) | xargs -n2 > users.txt
```

probably a good idea? Encrypting it even better? Sadly linux doesn't have good old ‘crypt’ for this, but gpg would do.

```sh
chmod 600 users.txt
```

Single call to pwgen

```sh
printf '%s\n' {01..50} | paste - <( pwgen -1 16 50 )
```

## ps Formatting and Ordering

Change the output of ps to be just memory, pid and process args, sorting the processes by memory usage (first column) and cutting the lines to fit the terminal.

```sh
ps -e -orss=,pid=,args= | sort -b -k1n | cut -c1-$COLUMNS
```

## Word Wrap a File

Wrap the lines of draft.txt at 72 characters wide, doing so at spaces, not middle of word (-s). Save the output to a new file draft-wrapped.txt

```sh
fold -w 72 -s draft.txt > draft-wrapped.txt
```

## zsh File Matching

```sh
vi *(.om[1]) # newest file o=order m=modified
ls *(.Om[1]) # oldest file ls
*(.oL[1]) # smallest file L=Size ls
*(.OL[1]) # largest file
ls -hlS /**/*(.OL[1,10]) # find the 10 biggest files on your system
gvim -p *(.om[1,3]) # open 3 newest files in tabs
```

## Drawing with xdotool

Move your mouse from the command line in a perfect spiral using the xdotool and polar coordinates, for example: in krita

```sh
x=0; y=0; while [[ $y -lt 500 ]]; do xdotool mousemove --polar $x $y; x=$(($x+4));y=$(($y+1)); sleep 0.01; done
```

## Show Listening Ports

Show all listening TCP/UDP ports for the current user. Or for everything if this is run as root.

```sh
lsof -Pan -i tcp -i udp
```

## Draw Horizontal Line

Define a function called hr that creates a horizontal line the width of your terminal when it's run. If you call it with an argument, the first letter of that argument will be used as the line draw character.

```sh
hr(){ printf '%0*d' $(tput cols) | tr 0 ${1:-_}; }
```

## Sort Processes

Show top 3 processes sorted by CPU usage

```sh
ps aux --sort=-pcpu | head -n 4
```

Show top 3 processes sorted by memory usage

```sh
ps aux --sort=-rss | head -n 4
```

## Find Port Occupier

So, how to determinate which process is listening on port 80:

```sh
$ netstat -tulpn | grep 80
tcp6       0      0 :::80                 :::*                   LISTEN     10177/java
```

`10177` is a pid you are looking for. Now execute

```sh
ps aux | grep 10177
```

for more process details.

## Port Forwarding

```sh
ssh -L{port on your PC}:localhost:{database's port} root@{server IP}
```

The command below will open port 3308 on your machine and everything will be forwarded to 192.168.1.2:3306

```sh
ssh -L3308:localhost:3306 root@192.168.1.2
```

localhost means that database is listening on 192.168.1.2. You can type for example 192.168.3.77 and everything will be forwarded to .3.77 server via .1.2

## Largest Files and Folders

List the 20 largest files and folders under the current working directory.

```sh
du -ma | sort -nr | head -n 20
```

With human-readable, give `-h` option to both

```sh
du -ha | sort -hr | head -n 20
```

## Arch Packages By Size

```sh
pacman -Qq | pacman -Qi - | egrep '(Size\s+:|Name\s+:[^s])' | sed -E 's/ ([KM])iB/\1/' | sed -z 's/\nInstalled/ /g' | perl -pe 's/(Name|Size) *: //g' | column -t | sort -hk2 -r | cat -n | tac
```

## Count Matching Lines In Files

Count the number of lines with a question per txt file and order them by count. Note that files with colons in their name will break this pattern.

```sh
grep -c '?' *.txt | sort -t: -k2rn
```

Can use sed to change delimiter for file names with colons

```sh
grep -c '?' &.txt | sed -E 's/:([0-9]+)$/~\1/' | sort -t~ -k2rn
```

## Fix Command

`fc` for 'fix command' opens the previous command in the $EDITOR. Edit, save, quit, and the command will execute

## Zip Old Files

gzip files in the current directory that are more than 60 days old.

```sh
find . -maxdepth 1 -type f -mtime +60 -exec gzip -v9 "{}" \;
```

cope if special chars in filename, add eg -P4 for parallel runs

```sh
find . -maxdepth 1 -type f -mtime +60 -print0|xargs -r0 gzip -v9
```

## Date Conversion

Using GNU date you can convert a Unix epoch time back into a human readable date by prefixing it with `@`

```sh
date -d @1539217802
```

## Show Significant Log Changes

Show temperature log lines when the temperature value changes more than one degree in either direction.

```sh
awk '{n=$1-lc;if (n<-1||n>1) { print; lc=$1; }}' temp.log
```

## List Only Directories

Show only the directories in the current directory. The `/` at the end of the glob wildcards make this trick work.

```sh
ls -d .*/ */
```

Show hidden before regular directories with `LANG=C`

```sh
LANG=C ls -d .*/ */
```

`-d` alone only shows current directory, likely because of default being reasonable in every other mode.

## Random Birth Dates

Create 50 random dates that are between 18 - 100 years ago. Used to create example birth date data. Not uniformly random; for example: you will get 6575 twice as often as 36363.

```sh
for i in {1..50} ; do d=$(( RANDOM % 29950 + 6575 )) ; date -d "$d days ago" +"%F"; done
```

## Reboot to UEFI Menu

```sh
systemctl reboot --firmware-setup
```

## Disable Screen Saver Blanking

```sh
xset s off
```

Add to `.xinitrc` as

```sh
xset s off &
```
