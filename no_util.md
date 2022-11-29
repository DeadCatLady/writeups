NAME: no_util
CATEGORY: misc
POINTS: 200
DOWNLOAD LINK: None
ACCESS: `ssh noutil@chal.ctf-league.osusec.org` (password is `osusec`)
DESCRIPTION: My friend says software bloat increases attack surface area, but this is a bit crazy...

You’re given a linux box with no access to busybox - meaning many typical linux commands won’t work, and you’re supposed to find the `flag.txt` file. So without using `find` or even `ls` we had to search for a file. 

The team I was in ended up rebuilding the `find` bash function, which searches through all the directories in the box. 

```bash
find() {
	for f in *; do
		if [[ "$f" == "flag.txt" ]]; then
			echo "$PWD: $(<$f)";
			return 0;
		fi;
		if [[ "$f !== ".."]] && [[ "$f" != "."]] && [[ "$f != "$1"]] && [[ $2 -ne 100]] && [[ -d "$f"]]; then
			cd "$f";
			find "$f" $(($2 +1)) && return 0;
			cd ..;
		fi;
	done;
	return 1;
}
```

Another way to do this - which is the most insulting thing ever, is just echoing out files, and digging deeper into the directory by doing the following:

```bash
echo `</*/*/*/*/flag.txt`
```
