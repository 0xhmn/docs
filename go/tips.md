### Reading and buffering
- to read a file at once we can use ioutil.ReadFile("fileAddress")
  - the problem with this approach is this will read the whole file at once (not ideal for big sizes)
  
- to counter that problem we can use `os` and `bufio`
  
### multilien string
- using `