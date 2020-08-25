# Parcel
Container for a message (Data and object references) that can be sent through an IBinder. A Parcel can contain both flattened data that will bbe unflatteneed on the other side of the IPC (using the various methods here for writing specific types, or the general Parcelable interface), and reference to live IBinder objects that will result in the other side receiving a proxy IBinder connected with the original IBinder in the Parcel

Note 
Parcel is not a a general-prpose sesrialization mechanism. This class (and the corresponding Parcelable API for placing arbitrrary objects into a Parcel) is designed as a high-performance IPC transport. As such, it is not appropriate to place any Parcel data in to persistent storage: changes in teh underlying implementation of any of the data in the Parcel can render older data unreadable. 

The bulk of the Pracel API revolves around reading and writing data of various types. There are six major classes of such functions  available. 

## Primitives

The most basic data functions are for writing and reading primitive data types. Most other data operations are built on top of these. The given dta is written and read using the endianess of the host CPU> 

## Primitive Arrays. 

There are a variety of methods for reading and writing raw arrays of primitive objects, which generally result in writing a 4-byte length followed by the primitive data items. The mthods for reading can either read the data into an existing array, or create an return  a new array.

## Parcelables 
The Parcelable protocol provides an extremely effiecient (but low-level) protocol for objects to write and read themeseleves from Parcels. You can use direct methods to read or write. These methods write both the class type and its data to the Parcel, allowing that class to be reconstructed from teh appropriate class loader when later reading. 

There are also some methods that provide a more efficient way to work with Parcelables:These methods do not write the class information of the original object intstead, the caller of thea read function must know what type to expect and pass in teh appropriate instead to properly construct the new object and read its data. To more efficient write and read a signle Parcelable object that is not null, you can directly call 

## Bundles. 

A special type-safe container, called Bundle, is availbel for key/value maps of heterogenous values. THis has many optimizations for imporoved performance when reading and wrigint data, and its type-safe API avoids difficult to debug type errors when finally marshalling the data contents into a Parcel. The methods to use are writeBundel(android.os.bundle), readBundle() and readBundle()

## Activate Objects
An unusual feature of Parcel is the ability to read and write active objects. For these objects the actual contents of the object is not written, rather a special token referencing the object is written. When reading the object back from the Parcel, you do not get a new instance of the object, but rather a handle that poperates on teh exact same object that was originally written.There are two forms of the active objects available. 

Binder objects are a core facility of Android's general cross-process communication system. The IBinder interface describes an abastract protocol  with a Binder object. Any such interface can be written in to a  Parcel, and upon readin g you will receive either the original object implmementing that interface or a special proxy implmementation that communicates calls back to the original object.


FileDescriptor objects, representing raw Linux file descriptor indentifiers, can be written and ParcelFileDescriptor objects returned to operate on teh original file descriptor. The returnedfile descriptor is a dup of the original file descriptor: the object and fd is differenct, bu t operating on the same underlying file stream, with the same position, etc. The methods to use are

