---
layout: default
title: VFS
---

~~~
This is a Specification of the File System Layer, sliced at the 
~~~
###FileSystemLayerAlg.vdmpp

{% raw %}
~~~
class FileSystemLayerAlg
types 
public
public
public 
public 
public
public 
public 
public 
public 
public 
public 

functions
public
private
private
private
public 
private
private
FS_Init_Table() == {|->};
private


static public

static public
public
public
types
end FileSystemLayerAlg

~~~{% endraw %}

###UseFileSystemLayerAlg.vdmpp

{% raw %}
~~~
class UseFileSystemLayerAlg
instance variables
values
  file : FileSystemLayerAlg`File = mk_FileSystemLayerAlg`File(
operations
public dummy: () ==> ()
traces
  T : sys.FS_Init_Main() ; (sys.dirName(<Root>) | 

end UseFileSystemLayerAlg

~~~{% endraw %}
