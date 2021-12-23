# Write-Ahead Log

Some people are curious about what happens to the logs if Ingester dies before they are completely flushed. Write-Ahead log is for such a situation!

Here are the features of WAL.

* Written in Ingester's disk space when it receives post requests.
* Not stop the ingestion flow, unlike typical WAL
* Run recovery process when an ingester starts
* Purge unused WAL files periodically

There are two important file types, which are "segment" and "checkpoint".

### Segment files

At first, let's start with "segment".

#### 1. A "segment" file is created and WAL is appended to it, with **raw data.**

![](../.gitbook/assets/image.png)

#### 2. A new segment file is created when its size reaches the limit

![](<../.gitbook/assets/スクリーンショット 2021-12-21 18.28.57.png>)

### Checkpoints

The second is what "checkpoint" is and how it works.

WAL mechanism purges segment files periodically and there is a goroutine for that.

#### 1. Force to advance to a new segment file

So that the older segment file won't be written anymore and deleting the writing segment file won't happen.

![](<../.gitbook/assets/image (7).png>)

#### 2. A checkpoint file is created based on the streams on memory.&#x20;

A chunk in that is constructed with memory chunk, which has "head" and "block" array as mentioned in the previous chapter.

It means that "checkpoint" is a snapshot of memory chunks at that time and it is compressed as well. (but head elements are not compressed either)

![](<../.gitbook/assets/スクリーンショット 2021-12-23 16.07.18.png>)

#### 3. Purge unused files

It deletes the files except the newest segment and checkpoint.

That's how the snapshot of unflushed chunks on memory at that time remains.

![](<../.gitbook/assets/スクリーンショット 2021-12-23 16.48.38.png>)

### How to recover from WAL

The recovery process is called in starting ingester process.



