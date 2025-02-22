# CBSD: tags and custom facts

## facts

When working with command like this: [jls](../jail/wf_jls_ssi.md), [jget](../jail/wf_jget_ssi.md), 
[bls](../bhyve/wf_bls_ssi.md), [bget](../bhyve/wf_bget_ssi.md), [xls](../xen/wf_xls_ssi.md), [xget](../jail/wf_xget_ssi.md) and so on, you see the values of the underlying **CBSD**.
However, you can supplement and create your own informative fields for your environments, thereby expanding the output of **CBSD**.

For these purposes, a _${workdir}/jails-system/${jname}/facts.d_ a directory is used, into which you can save an executable file under an arbitrary name.
The name of this file is your custom field and the information that your script will output will be available for the above scripts
will be available for the above scripts.
The value in the output should be in the format of one word (digits).

At the same time, inside your scripts, you will be available to the internal facts of environments from **CBSD**: [CBSD variables](http://www.convectix.com/en/13.0.x/wf_cbsd_variables_ssi.html).

For example:

```
% cat > ~cbsd/jails-system/jail1/facts.d/mycustom1 <<EOF
#!/bin/sh
echo "MYCUSTOM1"
EOF

% chmod +x /usr/jails/jails-system/jail1/facts.d/mycustom1
% cbsd jls display=jname,mycustom1
% cbsd jget jname=jail1 mycustom1
```

![](http://www.convectix.com/img/custom_facts1.png)

## tags

In addition to custom facts, you can use `tags` - this is a field in the SQLite3 database. This is the main difference between tags and facts - this value is static, while facts get the result in runtime.

Also, thanks to SQL, tags can participate in selections, e.g.:


`cbsd jls ip4_addr=10.0.0.1` is the same: `"SELECT * FROM jails WHERE ip4_addr="10.0.0.1"`

or

`cbsd jls astart=1 display=ip4_addr` is the same: `"SELECT ip4_addr WHERE astart=1"`

and so on:

![tags.png](https://convectix.com/img/tags.png)
