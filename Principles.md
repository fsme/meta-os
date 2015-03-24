# Meta OS Design Principles #

## Everything are file ##

Once upon a time in the medieval kingdom UNIX figured out for devices - this is also are files. It was very convenient. A long time after in the Plan9 far, far away galaxy has identified that the proceses - it is also are files.

Life has become even easier!

It is reasonable to continue this trend and make it an absolute principle. All the _objects_ of our _digital universe_ and all of the properties and content of these objects _are represented as files_ on a hierarchical file system.

If something does not to want to be a file, we will enforce it. There are no other resource, or other interface, or other properties, or other object except a file.

This also applies to all programming tools. Simple example. Almost any programming language can contain a statement:

> `X = 1`

What are this `'X'`? Each language offers his version of the content and properties of an object `'X'`.

MetaOS considers **X** as a file.

## Any file are directory ##

Complex type, more complex than basic, are represented as files, which lists all the contents of the type.

If **X** has a size, it can be found at **X/size**

If **X** has a comment, it can be found at **X/comment**

If **X** has a State named "wait", it can be found at **X/wait**

Any complex object consists of the enclosed complex subjects and embeded types, always having at the bottom a set of simple embeded types, like **byte**, **real** or **great**.

Content of file just joins to a files tree inside.

MetaOS is strictly adhere to this principle everywhere and in everything. No exceptions.

## Symbolic memory ##
The [Deus](Deus.md) has no direct addressed access to memory. All data are available by name only, by the file name. No exceptions.

## Private namespaces ##

All resources and services being used by a process are combined into a single rooted hierarchy of file names, called a `"namespace"`.

Each process builds a unique private view of the resources and services which it needs. Each set of resources is represent as files.

Resources uses completely transparently, remote as local. The resources accessible within an individual namespace can be located on a single client or on multiple servers throughout the network.

## Character set and representation ##

MetaOS uses the [UTF-8](http://plan9.bell-labs.com/magic/man2html/6/utf), which was [invented by Ken Thompson](http://www.cl.cam.ac.uk/~mgk25/unicode.html#history).

Not only for text files, and it is absolute for all data, including the bytecode of the [Deus](Deus.md) VM and any processing of binary files. All data are stored, transfered and processed in the coding UTF-8.

## Standard communication protocol ##

Meta OS uses service protocol to represent and access resources similar [9P2000](http://plan9.bell-labs.com/sys/man/5/INDEX.html) (a.k.a [Styx](http://www.vitanuova.com/inferno/papers/styx.html)), named [Cry](Cry.md).

Some changes are made because of change the permissions concept. [Cry](Cry.md) is reduction of _"Ciphering Resources Yet"_.

For simplicity of implementation of Meta-philosophy the protocol is included in CISC instruction set of the [Deus](Deus.md) VM.

## Ciphering security for ##

Encryption of all data by default makes it possible not to use "permits" old-style of centralized OS. In today's world of distributed computing and multi-data control "permission" has become a fiction and a burden that can be easily bypassed by hackers.

MetaOS has no concept of "permission". Each process are selfcare for the preservation of privacy. To gain access to the data of process is possible through the key of decryption only. To make changes to file is possible through protocol of encryption with signature only. No exception.

## No Users ##

MetaOS has no user database like '/etc/passwd' and does not have any analog.

The user reads only those files which may to decrypt his keys. The user can modify only those files which allows to encrypt his certificate. Meta OS has no need to store of names of users or a place to store of passwords.

The user can have one or more nicknames for compatibility _(example for e-mail)_, but these names will never be used for authentication.

## No Kernel ##

Meta OS has no kernel to implement the so-called 'system calls', which running with elevated privileges. All the processes running are equal. Meta OS does not use protection of memory through MMU or other way because direct access to memory does not exist. [Deus](Deus.md) has memory management buit-in.

## Robots for execution ##

Because in strictly order to distinguish between previous existing and our new development tools, instead of the term a "program", we use terms "robot" or "bot".

A Bot is Finite State Machine. States, Events, Action and all of them representation as files on the `Bot File System`, [Botfs](Botfs.md).

Language of bots is named [Kode](Kode.md). Executable byte-code of the [Deus](Deus.md) virtual machine is named [ByteKode](ByteKode.md). Mnemonic names of [Deus](Deus.md) instruction is named [MnemoKode](MnemoKode.md).