# bash prompt
## All Bash Prompt Variables
These are the environment variables provided by BASH (and most shells) and control your prompt string. While all are interesting and good to know about, PROMPT_COMMAND and PS1 are the only ones that directly modify the prompt that is displayed.

### PROMPT_COMMAND
- If set, the value is executed as a command prior to issuing each primary prompt.
### PS1
- The value of this parameter is expanded and used as the primary prompt string. The default value is "\s-\v\$ ".
### PS2
- The value of this parameter is expanded as with PS1 and used as the secondary prompt string. The default is "> ".
### PS3
- The value of this parameter is used as the prompt for the select command.
### PS4
- The value of this parameter is expanded as with PS1 and the value is printed before each command bash displays during an execution trace. The first character of PS4 is replicated multiple times, as necessary, to indicate multiple levels of indirection. The default is "+".

## Prompt Escape Codes
When executing interactively, bash displays the primary prompt PS1 when it is ready to read a command and the secondary prompt PS2 when it needs more input to complete a command. Bash allows these prompt strings to be customized by inserting a number of backslash-escaped special characters that are decoded as follows:

- \a - an ASCII bell character (07)
- \d - the date in "Weekday Month Date" format (e.g., "Tue May 26")
- \D{format} - the format is passed to strftime(3) and the result is inserted - into the prompt string; an empty format results in a locale-specific time - representation. The braces are required
- \e - an ASCII escape character (033)
- \h - the hostname up to the first '.'
- \H - the hostname
- \j - the number of jobs currently managed by the shell
- \l - the basename of the shellâs terminal device name
- \n - newline
- \r - carriage return
- \s - the name of the shell, the basename of $0 (the portion following the final - slash)
- \t - the current time in 24-hour HH:MM:SS format
- \T - the current time in 12-hour HH:MM:SS format
- \@ - the current time in 12-hour am/pm format
- \A - the current time in 24-hour HH:MM format
- \u - the username of the current user
- \v - the version of bash (e.g., 2.00)
- \V - the release of bash, version + patch level (e.g., 2.00.0)
- \w - the current working directory, with $HOME abbreviated with a tilde
- \W - the basename of the current working directory, with $HOME abbreviated with - a tilde
- \! - the history number of this command
- \# - the command number of this command
- \$ - if the effective UID is 0, a #, otherwise a $
- \nnn - the character corresponding to the octal number nnn
- \\ - a backslash
- \[ - begin a sequence of non-printing characters, which could be used to embed - a terminal control sequence into the prompt
- \] - end a sequence of non-printing characters

## Basic POWER PROMPT

```bash
PROMPT_COMMAND='history -a;echo -en "\033[m\033[38;5;2m"$(( `sed -n "s/MemFree:[\t ]\+\([0-9]\+\) kB/\1/p" /proc/meminfo`/1024))"\033[38;5;22m/"$((`sed -n "s/MemTotal:[\t ]\+\([0-9]\+\) kB/\1/Ip" /proc/meminfo`/1024 ))MB"\t\033[m\033[38;5;55m$(< /proc/loadavg)\033[m"' \
PS1='\[\e[m\n\e[1;30m\][$$:$PPID \j:\!\[\e[1;30m\]]\[\e[0;36m\] \T \d \[\e[1;30m\][\[\e[1;34m\]\u@\H\[\e[1;30m\]:\[\e[0;37m\]${SSH_TTY} \[\e[0;32m\]+${SHLVL}\[\e[1;30m\]] \[\e[1;37m\]\w\[\e[0;37m\] \n($SHLVL:\!)\$ 'h

```

```
PS1="\n\[\e[1;30m\][$$:$PPID - \j:\!\[\e[1;30m\]]\[\e[0;36m\] \T \[\e[1;30m\][\[\e[1;34m\]\u@\H\[\e[1;30m\]:\[\e[0;37m\]${SSH_TTY:-o} \[\e[0;32m\]+${SHLVL}\[\e[1;30m\]] \[\e[1;37m\]\w\[\e[0;37m\] \n\$ "
```
```bash

$ PS1="\n\[\e[32;1m\](\[\e[37;1m\]\u\[\e[32;1m\])-(\[\e[37;1m\]jobs:\j\[\e[32;1m\])-(\[\e[37;1m\]\w\[\e[32;1m\])\n(\[\[\e[37;1m\]! \!\[\e[32;1m\])-> \[\e[0m\]"

2515/4039MB     0.00 0.00 0.06 1/244 3457
(meirfi)-(jobs:0)-(~)
(! 69)->

```

## related links

* http://bashrcgenerator.com/