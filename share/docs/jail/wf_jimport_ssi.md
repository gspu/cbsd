# Jail import

## jimport command

```
	% cbsd jimport
```

**Description**:

Import jail from (\*.img) file.

In **jname** argument you can set full path to file or just filename (without **.img** extension) if file placed in _$workdir/import_ directory. **img**-file after import is not modified. In addition, you can import an image of a jail in an alternate name (for example, you want to deploy the image of the jail1 from **jail1.img**, however, your system already has a jail of the same name). In this case, an argument newjname can be used.

**Example:**

import jail from _/tmp/mysqljail.img_ file:

```
cbsd jimport jname=/tmp/mysqljail.img
```

**Example**:

import the image jail2.img placed in _$workdir/import_ with a new name **jail5**:

```
cbsd jimport jname=jail2 newjname=jail5
```

![](http://www.convectix.com/img/jimport1.png)


