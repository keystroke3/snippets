# snippets
A collection of small bits of code that make my life easier on linux
## Navigation
snippets that help you move around terminal quickly. Just add the snippet you want to `.bashrc` or `.zshrc`. For all the snippets here, it is best to have [FZF](https://github.com/junegunn/fzf) installed on your system. If you want to change the start location, simply change `$HOME` to whatever you want. Note that if you choose to start from root (/), it might take some time to work, since it looks through the entire system.
### Jump to any directory from any directory.

```shell
#!/bin/sh

j(){
    fuzz=`fd -Ht d . $HOME | fzf --reverse --height 40%`
    if [ -z $fuzz ]
    then 
        return 0
    elif [ -d $fuzz ]
    then
        cd $fuzz
    fi
}

```
Now type `j` and press enter. 

### Open any file from anywhere.
```shell
#!/bin/sh

o(){
    fuzz=`fzf --reverse --height 40%`
    if [ -z $fuzz ]
    then 
        return 0
    elif [ -f $fuzz ]
    then
        filetype=`xdg-mime query filetype $fuzz | awk -F/ '{print $1}'`
        case $filetype in
            text )
                vim $fuzz ;;
            audio )
               nohup mpv $fuzz ;;
            application )
                vim $fuzz ;;
            * )
                nohup xdg-open $fuzz > /dev/null 2>&1
        esac
    fi
}

```
typ `o` then enter. Vim is my text editor of choice and this is what I use here. If you wish to change or use a different one, just change `vim` to whatever text editor you want. If your text editor is a graphical one like Gvim or Gedit, you can should add `nohup` before like so:
```
text )
    nohup gedit $fuzz ;;
```
Nohup is there only to allow you to keep using you terminal after the program has lauched.

## File Manipulaton
### How to separate a list of files and directories stored in a text file

If you have a list of files and directories all in one text file and wan to seperate them to different files Here is what you can do 
in python:

```python
#!/bin/python3

working_dir = "Documents/teams/"
with open(working_dir+"original.txt", "r") as full_list:
    with open(working_dir+"only_dirs.txt", "a") as dirs, \
            open(working_dir+"only_files.txt", "a") as files:
        lines = full_list.readlines()
        for line in lines:
            if "." not in line[-6:] or line[-1:] == "/":
                dirs.write(line)
                print(f"... {line[-50:]} is a directory")
            else:
                files.write(line)
                print(f"... {line[-50:]} is a file")
```
Looking at the code above, we first start by specifiying the directory where our files are and where they will be stored. This can be ignored if you are using this python file in the same directory as the files in question. We the the `open` to open file with the list of paths and new files in append mode. We then read the paths file and store the contents in a `lines` variable. We then iterate through the the lines and check if the lines end with a file extension. The line `if "." not in line[-6:]:` just checks if the last few characters have a "." (dot) in them. This is because all file extensions are of the form `.*` where * can be any character or set of characters. Incase the line ends with `/` then it is treated as a directory.  
if a line has no file extension or ends with "/" then it is assumed to be a directory and is adde to the `only_dirs.txt` file. otherwise, it is placed in the `only_files.txt` file. The print statements are just for feedback and can be ignored.
