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
- Lang »  - [Русский](http://www.convectix.com/ru/13.0.x/wf_repo_ssi.html)
  - [English](http://www.convectix.com/en/13.0.x/wf_repo_ssi.html)
  - [Deutsch](http://www.convectix.com/de/13.0.x/wf_repo_ssi.html)

2020-10 upd: we reached the first fundraising goal and rented a server in Hetzner for development! Thank you for [donating](https://www.patreon.com/clonos) !

Attention! Current pages describe **CBSD** version **13.0.x**. If you are using an older version, please update first.

Attention! I apologize for the automatic translation of this text. You can improve it by sending me a more correct version of the text or fix html pages via [GITHUB repository](https://github.com/cbsd/cbsd-wwwdoc).

# Operaion with repository

## repo command

```
			% cbsd repo

```

```
			% cbsd repo-tui

```

**Description**:

Work with a repository of bases, kernels and images. The quantity a repository can be more than one and they are specified through a gap in the file repo variable _$workdir/nc.inventory_. Downloading will occur from the first repository where the object will find. Respectively, if the local repository is used — it should be the first.

obligatory parameters:

- **action** — can accept value _list_ (get list), _get_ (download), _put_ (upload)

Arguments which in certain cases aren't obligatory:

- **sources** — sources for **action** — with what data we want to work. Can accept values:

  - _src_ — source code for OS (${workdir}/src)
  - _obj_ — object file for source code ($workdir}/obj)
  - _base_ — world/bases ($workdir/base)
  - _kernel_ — kernels of OS (${workdir}/base)
  - _img_ — jails
- **name** — name (used with sources=obj,base,kernel,img — name of base/world or jails)
- **stable** — related to sources=obj,base — get RELENG\_X instead of RELENG\_X\_Y
- **ver** — By default, for obtaining the list or downloading the current version of OS will be used. With the ver=X.Y parameter it is possible to specify other version for jail/base/kernel. At ver=any for action=list, will be it is deduced all available data of sources for all versions

**Example**:

Obtaining the list of available jails for 9.1 versions

```
			% cbsd repo action=list sources=img ver=9.1

```

to get and import kfreebsd jail:

```
			% cbsd repo action=get sources=img name=kfreebsd

```

![](http://www.convectix.com/img/repo1.png)

Upon termination of an import the question of correct IP for a new jails will be asked and whether to create alias automatically. We choose COMMIT for preservation.

![](http://www.convectix.com/img/repo2.png)

Now jail in system also it is possible to use

![](http://www.convectix.com/img/repo3.png)

Copyright © 2013—2024 CBSD Team.

