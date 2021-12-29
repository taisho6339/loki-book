# How to retain data in an ingester

// TODO move to another page later

Here is how to retain data in an ingester.

![](<../.gitbook/assets/スクリーンショット 2021-12-23 17.13.12.png>)

* Memory chunk has "head" array, which element is raw data.
* Memory chunk has "block" array, which element is compressed.
* Segment file on the ingester disk is raw data
* Checkpoint on the ingester disk is the snapshot of memory chunks
