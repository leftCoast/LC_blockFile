# LC_blockFile
Tools for manipulating as SD file like RAM. Manages multiple blocks of data in a single file.  

**Depends on**  
[Arduino SD library](https://www.arduino.cc/en/Reference/SD)

**LC_blockFile overview :** The LC_blockFile library enables you create a blockFile object that manages a file like it was a gigantic heap of RAM.

The blockFile object associates itself to a single file on an SD card. A blockFile object is created with a fullpath to a file that it either creates, or has already been created.

A blockFile object can allocate new block IDs and, using those IDs, save blocks of data in its file. The amount of data blocks that can be allocted is nearly unlimited. Your blockFile object stores and manages of all your data blocks in its file.

To retrieve a block, just pass back the ID for you data, and your blockFile object will return how large that stored block is. Then, after you’ve allocated enough memory to hold it, using that same ID you can retrieve your data.

You can manipulate your data as you see fit. Maybe just change a few things, make it larger, smaller, whatever. Just pass it back with its new size & block ID. Your blockFile object will squirrel it away for you ‘till you need it again.

Or you can just tell your blockFile object to delete the ID. It’ll takes care of that as well.

One last thing..

If you really think about it, this is like trying to open the crate that encloses the crowbar you need to open the crate.  If all your information is stored in the blockFile. How do you know what your block IDs are?

The FIRST ID passed back from a blockFile object, is saved as its rootID. You can ask for the rootID and it’ll pass it back to you. The trick is to save, under this ID, the information necessary to decode the rest of your file.  


**How to use this?** A blockfile is created using a full path for a datafile. Either a blockfile, empty file or no file at all. If its no file at all, blockfile will create an empty one for you. **NOTE :** blockFiles can not be created globally. IE. Before the setup() function has been called. **blockfiles must be created after the SD hardware has been initilized.** If you need a global blockfile, and most do, declare a global pointer to a blockFile, then create the actual blockfile object after initialization of the SD library.  
```blockFile* ourFile;``` Global pointer..

```ourFile = new blockFile(fullPath);```  Creation after SD drive is initialized.  

**Saving a block of data.**

**1 :**  For saving a new block of data that's never been saved before, call the addBlock() method. This allocates a new block ID for the new datablock then saves the data block under that new ID. Upon completion the new ID is returned.  
```newID = ourFile->addBlock(dataBlockPtr,numBytes);```  

**2 :** To save a datablock that already has a data block ID assigned to it use the writeBlock() method. This method takes an ID and stores a datablock to it.  
```ourFile->writeBlock(blockID,buffPtr,numBytes);```  

**NOTE :** Use the first block allocated ( Known as the root ID. ) to store the information you will need to decode the rest of your file. Because you can always get back the root ID with the readRootBlockID() call.  

```ourRootID = ourFile->readRootBlockID();```  

**3 :** An alternate way to aquire a block ID, without having to save a datablock, is using the getNewBlockID() method. This will allocate a file ID that can later be used with the writeBlock() method.  
```
newID = ourFile->getNewBlockID();
success = ourFile->writeBlock(newID,dataPtr,numBytes);
```  

**Retrieving a block of data.**

**1 :** First step would be to see how many bytes this block of data is. Use the getBlockSize() method for this.  
```numBytes = ourFile->getBlockSize(blockID);```  

**2 :** After allocating a buffer large enough to fit the data block. Retrieve it using the getBlock() method.  
```haveBlock = ourFile->getBlock(blockID,dataPtr,blockID);```  


**Deleting a block of data.**

**1 :** Use the deleteBlock() method to delete a datablock out of a blockFile.  
```blockDeleted = ourFile->deleteBlock(blockID);```  







