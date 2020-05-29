# LC_blockFile
Tools for manipulating as SD file like RAM. Manages multiple blocks of data in a single file.


**LC_blockFile overview :** The LC_blockFile library enables you create a blockFile object that manages a file like it was a gigantic heap of RAM.

The blockFile object associates itself to a single file on an SD card. A blockFile object is created with a fullpath to a file that it either creates, or has already been created.

A blockFile object can allocate new block IDs and, using that ID, save blocks of data in its file. The amount of data blocks that can be allocted is nearly unlimited. Your blockFile object stores and manages of all your data blocks in its file.

To retrieve a block, just pass back the ID for you data, and your blockFile object will return how large that stored block is. Then, after you’ve allocated enough memory to hold it, using that same ID you can retrieve your data.

You can manipulate your data as you see fit. Maybe just change a few things, make it larger, smaller, whatever. Just pass it back with its new size & block ID. Your blockFile object will squirrel it away for you ‘till you need it again.

Or you can just tell your blockFile object to delete the ID. It’ll takes care of that as well.

One last thing..

So far, this is like trying to open the crate that encloses the crowbar you need to open the crate.  If all your information is stored in the blockFile. How do you know what your block IDs are?

The FIRST ID passed back from a blockFile object, is saved as its rootID. You can ask for the rootID and it’ll pass it back to you. The trick is to save, under this ID, the information necessary to decode the rest of your file.
