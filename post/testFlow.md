+++
categories = ["category1", "category2"]
date = "2016-05-24T18:37:21+08:00"
tags = ["tag1", "tag2"]
title = "testFlow"

+++

```flow

st=>start: Start
e=>end: End
op1=>operation: lwj1
sub1=>subroutine: lwjs
cond=>condition: succeed
io=>inputoutput: input1

st->op1->cond
cond(yes)->io->e
cond(no)->sub1(right)->op1

```

[x] dddd
