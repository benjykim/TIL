
#### ETC
| 명령어 | 설명 | 예시 | 
|---|---|---|
| ntpupdate | set the date and time via NTP | ntpupdate -b -u IP <br/> -b = Force the time(step mode) <br/> -u = If you are running ntpd, "-u" must be added |
| history | GNU History Library | history \| less <br/> history 5 ← lists only the last 5 lines <br/> HISTSIZE=1000 <br/> HISTTIMEFORMAT="%Y/%m/%d  %H:%M:%S" |
| which | locate a command | which ls |
| strace | trace system calls and signals | strace -t php test.php <br/> strace -t -o test.txt php test.php <br/> -t = each line of the trace with the time of day |
| ltrace | A library call tracer | ltrace -o test.txt wget http://example.com/ <br/> ltrace -p PID <br/> ltrace -p 3365 |
| script | make typescript of terminal session | script -afq $LOG |

### 파일 및 디렉토리 명령어
#### 파일 명령어
| 명령어 | 설명 | 예시 | 
|---|---|---|
| ls | list directory contents | ls -ltrh ← -r = reverse order while sorting <br/> -h = with -l, print sizes in human readable format <br/> ls -ltrh \| less |
| cp | copy files and directories | cp -p SRC DES <br/> cp -pr SRC/ DES/ ← -r, -R = copy directories recursively <br/> cp -f SRC DES ← -f, --force|
| mv | move (rename) files | mv file1 file2 <br/> mv dir1 dir2 <br/> mv file1 file2 DIR |
| rename | rename multiple files | rename 's/(.변경하고자 하는 text)/(.변경할 text)' (찾을 파일) <br/> rename 's/.txt/.c/' *.txt ← *.txt 파일을 모두 *.c파일로 변경하기 <br/> rename -n 's/test/ttt/' *.txt ← -n = 변경될 파일 미리 출력 가능(검증용)  <br/> rename -v 's/test/ttt/' *.txt ← -v = 변경 및 변경된 파일 출력 |
| rm | remove files or directories  | rm -rf TARGET ← -r, -R = remove directories |
| touch | change file timestamps | touch file1 <br/> touch -d "2021/10/20 13:00:00" file1 |
| ln | make links between files | ln -s SRC DES |
| unlink |  | unlink DES |
| wc | print newline, word, and byte counts for each file | wc -l ← -l, --lines = print the line counts |
| tree | list contents of directories in a tree-like format | tree -Dpuga /etc |
| col | filter reverse line feeds from input | man ifconfig \| col -bfx > test.txt |

#### 디렉터리 명령어
| 명령어 | 설명 | 예시 | 
|---|---|---|
| pwd | print name of current/working directory | pwd |
| cd | Change the current directory | cd ~ <br/> cd .. |
| pushd | Adds a directory to the top of the directory stack | pushd /var/log <br/> pushd \`pwd\` |
| popd | Removes entries from the directory stack | popd |
| dirs | displays the list of currently remembered directories | dirs -v |
| mkdir | make directories | mkdir -p /tmp/test1/test2/ ← make parent directories as needed <br/> mkdir -m 700 /home/user01/.ssh |
| rmdir | remove empty directories <br/> If you want to delete directory, you must use “rm -r DIR” | rmdir DIR |

#### 파일 내용에 접근하는  명령어
| 명령어 | 설명 | 예시 | 
|---|---|---|
| more | file perusal for crt viewing |  |
| less | opposite of more | crontab -l \| less |
| view | Start in read-only mode |  |
| cat | concatenate files and print on the standard output | cat /dev/null > access.log |
| tail | output the last part of files | tail -n 50 a.txt ← output the last N lines <br/> tail -f /var/log/messages |
| tailf | follow the growth of a log file | head -n 100 a.txt ← -n, --lines |
| head | output the first part of files |  |
| diff | compare files line by line | diff --suppress-common-lines --side-by-side File1 File2 <br/> diff -r dir1 dir2 ← When comparing directories, recursively compare |
| sdiff | side-by-side merge of file differences | sdiff -s File1 File2 ← -s = Do not print common lines <br/> sdiff -s -w 200 File1 File2 |
| colordiff |  |  |
| vimdiff |  | vimdiff File1 File2 <br/> vim -d File1 File2 |

#### 검색
| 명령어 | 설명 | 예시 | 
|---|---|---|
| grep | print lines matching a pattern | grep WORD FILE \| less <br/> grep -Ev "^#\|^$" xxx.tx <br/> grep . ifcfg-eth* ← check filename and contents <br/> grep -r PATTERN --include="*.txt" DIRECTORY ← -r = recursive  |
| egrep | egrep is the same as grep -E | egrep "aaa\|bbb" file |
| find, xargs | search for files in a directory hierarchy | find . -name "*txt*"  |
