<!-- 
.. title: Convert units with command line
.. slug: convert-units-with-command-line
.. date: 2017-02-15 13:38:55 UTC+07:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

It's fairly simple indeed.

<!-- TEASER_END: Read more -->
-------------------------

######Installation:
* On Ubuntu:
```
apt-get install units
```

* On Mac OSX:
```
brew install gnu-units
```
---------------------------------
##### Example:
1024 bytes to MB

> Ubuntu

`units -v 1024byte megabyte`

> Mac OSX

`gunits -v 1024byte megabyte`

> And the result:
```
1024byte = 0.001024 megabyte
1024byte = (1 / 976.5625) megabyte
```


