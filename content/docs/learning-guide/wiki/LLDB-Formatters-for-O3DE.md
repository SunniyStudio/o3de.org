---
title: "LLDB-Formatters-for-O3DE"
description: ""
toc: false
---

# LLDB Formatters for O3DE

When debugging O3DE on non-Windows platforms, the [LLDB](https://lldb.llvm.org/) debugger can be used to debug O3DE code.  
It tends to deal with memory issues with loading the o3de debug symbols better than GDB and is a lot faster at loading those debug systems.  

The [LLDB Python Scripting API](https://lldb.llvm.org/use/python.html) has been used to create formatters for several types provided with O3DE, mostly around the "AZStd" library.

## Download
The attached zip file contains the O3DE formatters for that can be used with O3DE.  
[lldb.zip](https://github.com/o3de/o3de/files/12705256/lldb.zip)

## Implemented formatters
The following types have implemented formatters and the files where their implementations are found 

| Types | Formatter Python Script |
|------|------|
|`AZStd::basic_fixed_string<.*>`| lldb/.lldb/o3destd_lldb.py|
|`AZStd::basic_string<.*>`| lldb/.lldb/o3destd_lldb.py|
|`AZStd::basic_string_view<.*>`| lldb/.lldb/o3destd_lldb.py|
|`AZStd::fixed_vector<.*>`| lldb/.lldb/o3destd_lldb.py|
|`AZStd::hash_table<.*>`| lldb/.lldb/o3destd_lldb.py|
|`AZStd::unordered_map<.*>`| lldb/.lldb/o3destd_lldb.py|
|`AZStd::unordered_multimap<.*>`| lldb/.lldb/o3destd_lldb.py|
|`AZStd::unordered_multiset<.*>`| lldb/.lldb/o3destd_lldb.py|
|`AZStd::unordered_set<.*>`| lldb/.lldb/o3destd_lldb.py|
|`AZStd::vector<.*>`| lldb/.lldb/o3destd_lldb.py|


## How to use

Download and uncompress the lldb zip file attached to the [Download](#Download) section above.  
The directory contains a `lldb/.lldbinit` file which can be used to source the o3de lldb python scripts to allow for a user's environment.
Underneath the `.lldb` in are the  python scripts that accesses the [LLDB Python Scripting API](https://lldb.llvm.org/use/python-reference.html) to add support for O3DE specific classes.

In order to use the LLDB formatters, they need to be sourced into the LLDB debugger.  
This can be done in multiple multiple ways.  
1. Manually using the LLDB script import command
    *  `command script import <path-to-o3destd-py>`
1. As part of an `~/.lldbinit` file that is placed in the user's home directory
    | OS | Path |
    | --- | --- |
    | Linux | `/home/<user>` |
    | MacOS | `/Users/<user>` |
    | Windows | `C:/Users/<user>` |

For more information on how to source an LLDB settings and commands into the debugger, follow the LLDB man page [Configuration Files](https://lldb.llvm.org/man/lldb.html#configuration-files)

The structure of files attached to this page is setup to add allow automatically registering the O3DE debug formatters with LLDb by copying the contents of the "lldb" folder into the user's home directory.  
**NOTE**: The contents of the "lldb" folder should be copied and not the the folder itself.  
i.e copy the ".lldbinit" file inside of the "lldb" folder to your home directory `~/` and not "lldb" folder.

### Structure of Home Directory after copying O3DE LLDB Python formatters
```
~/
    .lldbinit
    .lldb/
        o3destd_lldb.py
```

### Final Notes

* If you as a user already have other custom python formatters for use with LLDB, then the LLDB `command script import <path-to-o3destd-py>` command can be used to source the O3DE formatters into your lldb instance.
* On Linux/MacOS the `.lldbinit` file and `.lldb` directory are hidden due to starting with a \<dot>, so in order to view them in a file manager, it's hidden files option must be used. Also if copying from a file manager, the hidden files can't be selected without being visible.
