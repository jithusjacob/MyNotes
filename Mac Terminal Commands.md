## Mac Terminal commands
1. Print working directory
```pwd```
2. List files
```ls```
3. Change directory
```cd Home/Projects```
4. Show hidden files
```ls -a```
5. Long form of list files
```ls -l```
6. Long form of list files plus hidden files
```ls -la```
7. Go to parent directory
```cd ..```
8. Go to home directory
```cd``` or ```cd ~```
9. To create a directory
```mkdir testfolder ```
10. Create a file
```touch test.txt```
11. Open a file
```open test.txt```
12. Copy a file
```cp test.txt  copy_file.txt```
13. Move or rename(moving the file to the same directory with a different name).This will work for directory as well
```mv test.txt test_rename.tx``` 
14. Remove file from directory.This is permanent delete.
```rm test.txt```
15. Man command to check details about a command.To get out of there q.
```man cp```
16. Copy from one directory to another
```cp -R dir1/ dir2/```
17. To remove directory
```rm -R dir/```
18. List all the files including sub directories
```find .```
19. List all directories only and no files
```find . -type d```
20. List all files even those in subdirectories but no directories
```find . -type f```
21. Search for a file
```find . -type f -name "test.txt"```
22. Search for a file beginning with some text ex "test"
```find . -type f -name "test*"```
23. Search for a file beginning with some text ex "test".But not case sensitve
```find . -type f -iname "test*"```


