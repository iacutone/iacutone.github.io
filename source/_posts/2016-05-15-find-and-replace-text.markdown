---
layout: post
title: Find and Replace Text
date: 2016-05-15 12:28:24 -0300
comments: true
categories: ack grep
---

If you find yourself in the situation of needing to find and replace text in multiple files, use the Command Line Interface. I wanted to rename a Phoenix application and ran the below command.

`ack pivotal_commentor -l | xargs sed -i '' 's/pivotal_commentor/commentor/g'`

Let's break this command down with a simple example.

```
mkdir test
cd test
touch file1.txt
touch file2.txt
```

And lets add the text 'hello' to both text files.

```
ack hello -l
file1.txt
file2.txt
```

ack is like grep and found the files containing the string hello. The -l flag "Only print filenames containing matches"

```
ack hello -l | xargs
file1.txt file2.txt
```

xargs is a Unix utility that constructs argument lists and is smashing the filenames into one line.

Finally, we pipe the arguments from xargs to sed.

```
ack hello -l | xargs sed -i '' 's/hello/bye/g'
```

The -i flag allows for in place editing on the file. In the regex, the s replaces hello with bye and the g indicates globally, in case hello is found more than once in the file. The '' is sending the change into the correct output file.




