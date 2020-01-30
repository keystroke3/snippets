# snippets
A collection of small bits of code that make my life easier on linux
## Navigation
snippets that help you move around terminal quickly. Just add the snippet you want to `.bashrc` or `.zshrc`. For all the snippets here, it is best to have [FZF](https://github.com/junegunn/fzf) installed on your system. If you want to change the start location, simply change `$HOME` to whatever you want. Note that if you choose to start from root (/), it might take some time to work, since it looks through the entire system.
### Jump to any directory from any directory.

```shell
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

