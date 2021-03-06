# Introduction

### MongoDB

-   MongoDB is a open-source document database and leading NoSQL database with the scalability and flexibility that you want with the querying and indexing.

### MongoDB Atlas

-   MongoDB Atlas provides an easy way to host and manage your data in the cloud. MongoDB Atlas is a fully-managed cloud database.

---

Ever wondered how to store large files like pdfs or images into databases ? If you have basic knowlegde of MySQL (A relational database), then you might know there is **blob** datatype which can be used to store pdfs. But how to do the same in MongoDB ?

MySQL is relational database and hence it is not recommended to use for storing large amount of files, instead many professionals use MongoDB (A document-oriented NoSQL) database for it.

Curious about knowing the difference between Relational & NoSQL databases? [Click here](https://www.geeksforgeeks.org/difference-between-sql-and-nosql/)

---

<p align="center">
    <img src="images/upload.jpg" width="500" height="350">
</p>

## **Think, Think, Think....**

Do you know what is the maximum file size of any **BSON**-document that can be uploaded on MongoDB ?

You may think what's this **BSON** at first place ?? You might have heard of JSON format but not BSON right ? Let's have some basic understanding of it first.

-   JSON is a human-readable standard file format mainly used for transmission of data in the form of key-value attribute pairs.

-   BSON(Binary file format Type) is a serialization format encoding format for JSON mainly used for storing and accessing the documents.

Curious about knowing their differences and use ? Don't get confused with BSON & JSON. [Click here](https://www.geeksforgeeks.org/difference-between-json-and-bson/)

Okay, so now let's get back to our question ..

The answer is **16MB**. Ya !! only 16MB of BSON-document.
Now you might wonder how to upload files which are larger than 16MB as now-a-days the camera's HDR images themselves are 5-6MB big or pdfs which we read generally are 30-40MB in size atleast.

### Now, the solution for storing big files is using **GridFS with MongoDB**.

### **GridFS**

-   GridFS is the MongoDB specification for storing and retrieving large files such as images, audio files, video files, etc. GridFS has the capability to store files even greater than its document size limit of 16MB.
-   GridFS divides a file into chunks and stores each chunk(piece) of data in a separate document, each of maximum size 255K.

---

# Pre-requisites

1. Python3 should be installed on the system
2. Basics of database system
3. MongoDB basics
4. Basic Python knowledge

Follow this [link](https://realpython.com/installing-python/#how-to-install-python-on-windows) for Windows installation.

Follow this [link](https://realpython.com/installing-python/#how-to-install-python-on-linux) for Linux installation.

Follow this [link](https://realpython.com/installing-python/#how-to-install-python-on-macos) for MacOS installation.

---

# Activities

### Activity 1: Setting up MongoDB cluster using MongoDB Atlas

[Follow the instructions in the document](https://docs.google.com/document/d/1R03KKVmihUU7nrjdTSZfwj6qLB-UvvR8OybtpjRWtFc/edit?usp=sharing)

---

### Activity 2: Import the libraries which will be required for our tasks:

-   This microbyte uses Python language for performing tasks.
-   Firstly open terminal and copy the following snippet & paste it in the terminal.

```bash
    $ pip install pymongo gridfs bson dnspython
```

1. Import MongoClient so that we can connect to a running mongod instance on our cluster.
2. Import GridFS from gridfs module so that we may put, get and delete files in MongoDB.
3. Import objectid from the bson library because we will be requiring it.
4. dnspython package is needed to make network calls to the cluster.

-   Open the command prompt and type **python** and click enter (This will turn your cmd into python shell):

```python
from pymongo import MongoClient
from gridfs import GridFS
from bson import objectid
import dns
```

---

### Activity 3: Connect to the cluster and access the database

Now, we have imported the libraries, our task is to connect to the online cluster through our system.

**Task 1: Get the connection string to your cluster & connect to your database**

<details>
    <summary>Hint</summary>
While performing the Activity 1 above, you might have encountered with the connection string to your cluster and wondered about the use of it. This connection string is unique for every individual's cluster. Copy that connection string. Now use MongoClient to connect to it.

```python
connection_url = "mongodb+srv://CrioBytes:CrioBytes@criobytes.dyseu.mongodb.net/CrioBytes?retryWrites=true&w=majority"
conn = pymongo.MongoClient(connection_url)
# dbname will be present in your url (default db) named `test`
dbname = 'CrioBytes'
db = conn[dbname]
```

</details>
<br>

---

### Activity 4: Lets get started with interacting with our db & clear some basics

**Task 1: Create the GridFS object**

-   **objects** ! The entire universe is made of objects !!!
-   Here, I refer object as creating the instance of GridFS class i.e. `GridFS()`.
-   `GridFS()` takes an argument which is the object (oops! again an object) of db which we created in Activity 3.

<details>
    <summary>Hint</summary>

```python
    gridfs = GridFS(db)
```

</details>

<br>

**Task 2: Store simple string "Microbytes" in the database using gridfs**

-   Use `put()` method of gridfs instance we created to insert "Microbytes" in db
-   The `put()` method returns the object id of newly inserted object (may it be string or any file)
<details>
    <summary>Hint</summary>

```python
    obj = gridfs.put('MicroBytes')
```

</details>
<br>

> **Note: This string is stored in `fs` namespace which is the default namespace used for storing data using GridFS**

Curious about knowing what is **namespace** ?

-   A namespace is a system to have a unique name for each and every object.
-   At times, an object might be a variable or a method.
-   Lets break the keyword **namespace** as name (which means name, an unique identifier) + space(which tells us something related to scope). Here, a name might be of any method or variable and space depends upon the location from where is trying to access a variable or a method.
    <br>

---

### Activity 5: Diving deeper into GridFS

**Task 1: Create your own namespace**

-   Connect to cluster and create your own namespace while instantiating gridfs object
-   The `GridFS()` can also take another parameter which is the name of the **namespace** we wanna create.

<details>
<summary>Hint</summary>

```python
    fs = GridFS(db, "microbytes")
```

</details>
<br>

**Task 2: Insert any pdf file using gridfs**

-   Now, for inserting any file, we must have a reference pointing to that file.
-   In Python, we use `open()` function to open any file. This method returns a reference object to the file we opened.

```python
    f = open('/path_to_your_file/sample_bytes.pdf')
```

-   Use this reference object for inserting our pdf file.

<details>
<summary>Hint</summary>

```python
    file_id = fs.put(f, content_type='application/pdf', filename='sample_bytes.pdf')
    # you can provide the filename if you want to upload it with different name
```

Here, The put() method returned the file_id.

</details>
<br>

Now if you go to the database 'CrioBytes' in your cluster you fill find the collections named 'microbytes.files' & 'microbytes.chunks'

Now, there's some real crux & beatiful concept behind storing large documents using GridFS.

Let's dive deeper in understanding it....

-   You might know, that any database system must satisfy the ACID properties, where **A** stands for atomicity which is one of the most crucial thing to satify.

-   You might also know that, in MongoDB we can embed the documents.

-   In MongoDB, an operation on a single document is atomic (since we mongodb is document-oriented database).

-   But for situations that require atomicity of reads and writes to multiple documents (in a single or multiple collections), MongoDB supports multi-document transactions. With distributed transactions, transactions can be used across multiple operations, collections, databases, documents, and shards.

-   Multi-document transactions also are atomic (i.e. provide an “completely-or-nothing” logic).

-   When a transaction commits, all changes done to any data items in the transaction are saved and visible outside the transaction. That is, a transaction will not commit some of its changes while rolling back others.

-   Now, until a transaction commits, the data changes made in the transaction are not visible outside the transaction. But when a transaction writes to multiple shards(replica of entire data a cluster contains), not all outside read operations need to wait for the result of the committed transaction to be visible across the shards.

-   There can be large number of transactions which want to simply read the data, and very few transactions which want to write to these shards.

-   Now, we can see this problem can drastically reduce the performance of database system if more read operations and less write operations occur.

-   **This problem of any database system is quite famous which is basically due to maintaining atomicity(A) and at the same time providing concurrency(C).**

-   But in most cases, the multi-document transaction incurs a greater performance cost over single document writes.

-   To remove this drawback, GridFS does not support multi-document transactions.

-   Instead of storing a file in a single document, GridFS divides the file into parts, or chunks, and stores each chunk as a separate document.

-   GridFS uses two collections to store files. One collection stores the file chunks(`fs.chunks`), and the other stores file metadata(`fs.files`). Remember, we mentioned these two collections above.

Let's have a an understanding of the file is actually stored in these chunks:

1. Each document in the chunks collection(`fs.chunks`) represents a distinct chunk of a file as represented in GridFS.

```
{
  "_id" : The unique <ObjectId> of the chunk.,
  "files_id" : The _id of the “parent” document, as specified in the files collection.,
  "n" : The sequence number of the chunk. GridFS numbers all chunks, starting with 0,
  "data" : The chunk’s payload(data) as a BSON binary type.
}
```

2. The default chunk size in MongoDB is 64 megabytes.

3. A chunk consists of a subset of sharded data. Each chunk has a inclusive lower and exclusive upper range based on the shard key.

<p align="center">
    <img src="images/chunk.svg">
</p>
4. MongoDB splits chunks when they grow beyond the configured chunk size. A chunk may be split into multiple chunks where necessary. Inserts and updates may trigger splits.

<p align="center">
    <img src="images/chunk_split.svg">
</p>

[Click Here for indepth explanation](https://docs.mongodb.com/manual/core/sharding-data-partitioning/)

**You might wonder when should one actually use GridFS ?**

1. If your filesystem limits the number of files in a directory, you can use GridFS to store as many files as needed.

2. When you want to access information from portions of large files without having to load whole files into memory, you can use GridFS to recall sections of files without reading the entire file into memory.

3. When you want to keep your files and metadata automatically synced and deployed across a number of systems and facilities, you can use GridFS. When using geographically distributed replica sets, MongoDB can distribute files and their metadata automatically to a number of mongod instances and facilities.

**Task 3: Read the uploaded file using the file_id object**

-   Can you use `get()` & `read()` method provided by `fs` object.
-   `get()` method returns a file-like-object, and to see the content, we can call `read()` method on this file-like-object.
-   Will `get()` method require some parameter associated with the file we wanna retrieve? Ofc yes ! To find the file we want, it takes in the file_id.

<details>
<summary>Hint</summary>

```python
    print(fs.get(file_id).read())
    # This will print the content of the file which has id as file_id
```

</details>
<br>

**Task 4: Let's insert txt file into MongoDB**

-   Since you have worked with inserting pdf files, this task shall not be difficult one.
-   You will perform something just like below :

```python
    f = open('/path_to_your_file/sample_bytes.pdf')
    fs.put(f, filename="sample_text.txt")
```

-   What if we want to insert the file which is not present on the local system ? Can we insert some text file by inputting the data into that text file on the fly ? (dynamically) !! This might be necessary in some crucial tasks !!! So, lets see how to do so !

-   You might remember, we inserted the simple string "MicroBytes" at the start, and in subsequent task, we inserted text file. Now, the solution is simple, combine/tweak both ?

```python
    fs.put("Hello, This is a sample string", filename="new.txt")
```

<br>

**Task 5: Find a file in our database & read its contents**

-   As mentioned Task 2 of Activity 5, the files are stored in db.namespace.files collection. Use this collection to find the file stored.
-   Use `find_one()` method to find the document inside the collection.
-   Use `get()` & `read()` to read the contents of file.
<details>
    <summary>Hint</summary>

```python
    file_id = db.microbytes.files.find_one({"filename" : "sample_text.txt"},{"_id":1})
    print(fs.get(file_id['_id']).read())
    # Contents of the file `sample_text.txt` will be printed
```

</details>
<br>

**Task 6: Delete the file**

-   Now, suppose we want to delete the stored file, then use `delete()` method of `fs` object.
-   `delete()` method will also require the file_id of the file one wants to delete (Remember ! The `get()` method also needed file_id)
<details>
    <summary>Hint</summary>

```python
    fs.delete(file_id)
```

</details>
<br>

---

## Challenge activity

### Activity 6: Take this challenge & crack it ! (Yes you can do it! Believe yourself)

**Task: Upload the same file say `abc.txt` to the database thrice ! What do you think, will the file `abc.txt` will always be overwritten everytime you upload it or will it maintain copies of these files (versioning files) ?**

Hint: Why would someone not want the every version of a particular file ?

<br>

<details>
    
    The answer is MongoDB keeps every copy of the uploaded file on the database i.e. every version of a particular file uploaded multiple times is maintained.

    Let's analyse & understand a small scenario:

    You may upload same file multiple times to some website. A very common example is uploading your Resume with same name on particular job website multiple times after doing updation or adding some content to it. Now think you applied to some job with 'version1' of your Resume to say 'jobA' and then updated with it(lets say 'version2') and applied to 'jobB'. Now, what will happen if the recruiter of jobA sees the 'version2' of you resume. Is that logical ? It may or may not be. You might have made a changes that suits for jobB, and previous version had content suited for jobA particularly.

    If the database system would have overwritten your previous file, then the jobA's recruiter will find that your resume is not suited for that job role. Oh !! That seems to be an issue then. Well, that's why versioning is done !! Do not lose the old content, and have the new content too...

    One famous example which does versioning is the developer's love Github.

    Phew ! That is too much of background ? Let's see how MongoDB maintains version of files !!

</details>
<br>
Let's insert `abc.txt` thrice. Have a look at following code and unserstand how can be get the versions of same file:

```python
    f = open('abc.txt')
    # inital contents of 'abc.txt' : "initial"
    fs.put(f, filename='abc.txt')
    f.write('change1')
    fs.put(f, filename='abc.txt')
    f.write('change2')
    fs.put(f, filename='abc.txt')
    # Never forget to close the file if you are not using it.
    # Leaving the files open may lead to resource leakage.
    f.close()
    # retrieve all the files stored using find() function
    db.microbytes.files.find()
```

The output will be as follows:

```

{ "_id" : ObjectId("54e44bcdd6e6fe5c5c2b7174"), "chunkSize" : 171310, "filename" : "abc.txt", "length" : 7, "uploadDate" : ISODate("2020-10-31T08:22:37.029Z"), "md5" : "84e7d1b222bbf179b91e4284996dfcb2" }
{ "_id" : ObjectId("54e44bced6e6fe5c5c2b7175"), "chunkSize" : 171310, "filename" : "abc.txt", "length" : 7, "uploadDate" : ISODate("2020-10-31T08:22:38.958Z"), "md5" : "2cbcaf71ea3ff273dea2b8f32daf88e3" }
{ "_id" : ObjectId("54e44bcfd6e6fe5c5c2b7176"), "chunkSize" : 171310, "filename" : "abc.txt", "length" : 7, "uploadDate" : ISODate("2020-10-31T08:22:39.878Z"), "md5" : "58b88f4c74d30e50e0da0d72c1cd480c" }

```

You can see it has done versioning of the file.

-   `get_last_version()` method can be used to get the latest version of a file.

```python
    f = fs.get_last_version(filename="abc.txt"))
    # Read the content of file
    print(f.read())
    # print the upload date & time of the file (this will print the upload time of file reference we got above i.e. f)
    print(f.uploadDate)
```

The output is:

```bash
    >>> change2
    >>> datetime.datetime(2020, 10, 31, 8, 40, 47, 629000)
```

---

<br>

<p align="center">
    <img src="images/congrats.png" width="400" height="400">
</p>

<br>

Hooray 🎉🎉 We have successfully setup clusters, created database,uploaded, retrieved, deleted files using GridFS file system it through our personal laptop/desktop.

# Summary

In this microbyte,

1. Cluster & Database creation on MongoDB
2. Why GridFS & limitation of BSON type
3. How to use GridFS for basic upload
4. How MongoDB stores files actually on the clusters
5. Versioning of files in MongoDB

# References

1. https://docs.atlas.mongodb.com/getting-started/
2. https://www.w3schools.com/python/python_mongodb_getstarted.asp
3. https://www.tutorialspoint.com/mongodb/mongodb_gridfs.htm
4. https://psabhay.com/posts/mongodb/mongodb-gridfs-using-python/
5. https://docs.mongodb.com/manual/core/gridfs/
