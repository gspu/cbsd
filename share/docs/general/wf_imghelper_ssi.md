[View source on GitHub](https://github.com/cbsd/cbsd)

FreeBSD virtual environment management and repository

- [About](http://www.convectix.com/en/about.html)
- [News](http://www.convectix.com/en/news.html)
- [Screenshots](http://www.convectix.com/en/screenshots.html)
- [Tutorial](http://www.convectix.com/en/tutorial.html)
- [Documentation »](http://www.convectix.com/en/docs.html)  - [Articles by author's](http://www.convectix.com/en/articles.html)
  - [Articles and press](http://www.convectix.com/en/press.html)
- [Marketplace(Templates)](https://marketplace.convectix.com)
- [Support the project](http://www.convectix.com/en/donate.html)
- [bhyve.cloud](http://www.convectix.com/en/bhyve-cloud.html)
- Lang »  - [Русский](http://www.convectix.com/ru/13.0.x/wf_imghelper_ssi.html)
  - [English](http://www.convectix.com/en/13.0.x/wf_imghelper_ssi.html)
  - [Deutsch](http://www.convectix.com/de/13.0.x/wf_imghelper_ssi.html)

2020-10 upd: we reached the first fundraising goal and rented a server in Hetzner for development! Thank you for [donating](https://www.patreon.com/clonos) !

Attention! Current pages describe **CBSD** version **13.0.x**. If you are using an older version, please update first.

Attention! I apologize for the automatic translation of this text. You can improve it by sending me a more correct version of the text or fix html pages via [GITHUB repository](https://github.com/cbsd/cbsd-wwwdoc).

# How does a helper for **CBSD** image

## command: imghelper

```
			% cbsd imghelper

```

**Description**:


Prebuilt images for **CBSD** represent a archive of environment and a sequence scenario, which will be formed by one or another configuration derived from the image of the environment

Since most of the modifications associated with personal data (passwords, user names or domain name databases, etc.), to mining scenario must be received all necessary parameters

Starting with version **CBSD** 10.1.0, enter the settings dialog executes the script _imghelper_, which takes parameters for the construction of forms of SQLite3 database that runs along with the jail

In this paper we consider the construction of the classical dialog-based menu

SQL schema file format, the following (described format is used for testing **CBSD** updatesql:

```
CREATE TABLE forms (  idx INTEGER PRIMARY KEY AUTOINCREMENT, param TEXT DEFAULT NULL UNIQUE, \
desc TEXT DEFAULT NULL, defaults TEXT DEFAULT NULL, mandatory INTEGER DEFAULT 0, \
attr TEXT DEFAULT NULL, xattr TEXT DEFAULT NULL  );

```

Where:

- **forms** \- is a constant, a table name that imghelper will look when opening a file forms
- **idx** \- table index. When building forms are not used
- **param** \- Arbitrary name argument (usually - one word), a parameter whose value we should get
- **desc** \- Any description for the argument
- **defaults** \- The proposed default value. May be empty.
- **mandatory** \- boolean (0,1), a sign of commitment. If the parameter is mandatory, will not be able to run the script with an empty argument value
- **attr** \- System field for different attributes in this release/document it will not use
- **xattr** \- System field for different attributes when building WEB/HTML forms, in this version/documentation it will not use

For example, for wordpress from **CBSD** repo generation base occurs so: [initforms.sh](https://github.com/cbsd/cbsd-scenes/blob/master/img/wordpress/wordpress/bin/initforms.sh)

When **cbsd imghelper** executed specifies the path to the file (in the preparation of image through cbsd repo, the operation is done automatically).

There are three ways to enter the required parameters **cbsd imghelper** before it will run the installation script:

- Interactive mode: used to draw the dialog UI, parameters and input fields. By filling that button "COMMIT" initializes the install script
- Interactive and non-interactive, Method 1: Specify the parameters in the command line: cbsd imghelper param1 = val1 param2 = "this is arg for param2" ... In this case, if all the fields have values​​, the script will run automatically
- Interactive and non-interactive, methods are exactly 2: specify parameter values ​​using environment variable species H\_param. This method can also kobminirovatsya interactive mode when the environment variables will act as a "writable" or default values ​​in the dialog, allowing you to build a partially completed forms

Non-interactive mode is useful for the installation of the cell automatically, without interruption for input

A practical example

Create a file with a form to enter the 4 parameters: _username, password, dns1, dns2_. To do this, create an empty table in the file /tmp/forms.sqlite:

```
% sqlite3 /tmp/forms.sqlite
sqlite> CREATE TABLE forms (  idx INTEGER PRIMARY KEY AUTOINCREMENT, \
param TEXT DEFAULT NULL UNIQUE, desc TEXT DEFAULT NULL, defaults TEXT DEFAULT NULL, \
mandatory INTEGER DEFAULT 0, attr TEXT DEFAULT NULL, xattr TEXT DEFAULT NULL  );
sqlite> ^D

```

Fill in the table we need parameters

```
% sqlite3 /tmp/forms.sqlite << EOF
INSERT INTO forms ( param,desc,defaults,mandatory,attr ) VALUES ( "username","Please enter user name","oleg",1, "maxlen=10" );
INSERT INTO forms ( param,desc,defaults,mandatory,attr ) VALUES ( "password","Please enter password","",1, "maxlen=15" );
INSERT INTO forms ( param,desc,defaults,mandatory,attr ) VALUES ( "dns1","Please enter DNS1","8.8.8.8",1, "maxlen=15" );
INSERT INTO forms ( param,desc,defaults,mandatory,attr ) VALUES ( "dsn2","Please enter DNS2","",1, "maxlen=15" );
EOF

```

As you can see, all the fields are mandatory. Thus, the value of the parameters _username_ and _dns1_ The default is predetermined and offers equal _oleg_ and _8.8.8.8_ respectively

Run imghelper and see our field:

```
% cbsd imghelper /tmp/forms.sqlite

```

![](http://www.convectix.com/img/imghelper1.png)

Also, we can determine in advance the parameters via the command line (after having received the names of the variables in terms of --help):

```
% cbsd imghelper /tmp/forms.sqlite --help
[sys] Ncurses-based jail image boostrap helper
require: formfile
opt:  username password dns1 dsn2
External help: /usr/local/share/doc/cbsd/wf_imghelper.html
		% cbsd imghelper /tmp/forms.sqlite username=gelo dns1="1.2.3.4"

```

![](http://www.convectix.com/img/imghelper2.png)

Finally, we can simply use the environment variables:

```
% setenv H_username root
% setenv H_password strong_plain_text_password
% setenv H_dns1 192.168.1.1
% setenv H_dsn2 10.0.0.1
% cbsd imghelper /tmp/forms.sqlite

```

![](http://www.convectix.com/img/imghelper3.png)

Copyright © 2013—2024 CBSD Team.

