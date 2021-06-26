### File文件的构造器

**public File(String pathname)** 

>    以pathname为路径创建File对象，可以是**绝对路径或者相对路径**，如果pathname是相对路径，则默认的当前路径在系统属性user.dir中存储。
>
> > 绝对路径：是一个固定的路径,从盘符开始
> >
> > 相对路径：是相对于某个位置开始

**public File(String parent,String child)**

> 以parent为父路径，child为子路径创建File对象。 

 **public File(File parent,String child)**

> 根据一个父File对象和子文件路径创建File对象