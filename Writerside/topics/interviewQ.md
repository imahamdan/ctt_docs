# interviewQ

Given an array of size n, all cells are initialized to the value 0

## Input
a csv file that contains 3 numbers, a, b and k, each row describe start position, end position and addition to perform to the cells

Example
```
a,b,k
0,4,3 // from  0 .. 4 add 3
3,7,7 // from  3 .. 7 add 7
5,8,1 // from  5 .. 8 add 1
```

## Task
Write a program that perform the following
1. write a function that gets a file path and return the data
2. write a function that perform the addition actions described in file
3. return the MAX number in the Array  

## Notes
* write the program using psedu code
* you can assume you have open_file(path) and read_lines() methods
