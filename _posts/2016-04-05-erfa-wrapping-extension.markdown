---
layout: post
comments: true
title:  "ERFA Wrapping Extension"
date:   2016-04-05 20:00:00 +0530
author: Anchit Jain
category: code
tags: gsoc erfa
---
Hi everyone,

This year I am applying for [Google Summer of Code][gsoc] under [OpenAstronomy][openastro] umbrella organization to [Astropy][astropy]. My project idea is Implement Public API for ERFA. You can view my proposal in the previous post [here][proposal].

In this blog post, I am going to explain about ERFA and its use in Astropy.

ERFA is a C library containing key algorithms for astronomy and is based on the [SOFA][sofa] library published by the [International Astronomical Union][iau] (IAU). It implements high-quality astronomical routines for coordinate, time etc. More about ERFA can be seen [here][liberfa].

In Astropy `_erfa` is a python interface, not publically exposed, which wraps around original C functions, creating and exposing vectorized versions of them.
Main parts in `_erfa` are  Jinja 2 templates `core.c.templ` and `core.py.templ`. These combined with `erfa_generator.py` generates the wrapper code, splitting the wrappers into python portion `core.py` and C portion `core.c`.

The main goal of my project is to pull out pull this wrapper out of astropy and instead make a standalone Python wrapper library so that any project can use it without needing Astropy.

To get to know the extent to which the modifications in existing wrapper would be needed I tried to do some quick *hacks* on it.  

Currently `astropy._erfa` wraps only from `Astronomy` section and `AngleOps`, `SphericalCartesian` subsections under `VectorMatrix` section from the original erfa library. This could be checked in `erfa_generator.py` [here][erfa_generator].

To test how the current infrastructure works if it is wrapped to all the sections of original erfa library I modified above code to always `True` condition

``` python
if True:
```

and compiled the whole project again.
And it failed. The error was:

```
astropy/_erfa/core.c: In function ‘Py_cr’:
astropy/_erfa/core.c:5376:9: error: too few arguments to function ‘eraCr’
eraCr(*_r);
^
In file included from astropy/_erfa/core.c:14:0:
cextern/erfa/erfa.h:376:6: note: declared here
void eraCr(double r[3][3], double c[3][3]);
^
error: command 'x86_64-linux-gnu-gcc' failed with exit status 1
```

Above error suggests that `eraCr` function was not wrapped correctly, which would need changing the templates.

To test for other remaining sections I modified the code to

``` python
if not (subsection == "CopyExtendExtract"):
```

This time, it compiled successfully and the whole package was set up.
I tried to use some functions like `eraPmp` etc. and they all worked correctly. But it all needs to be checked carefully.

From this little exercise, I was able to somewhat understand the workflow in `_erfa` and relate to the changes the project would be needing.

[gsoc]: https://summerofcode.withgoogle.com/
[openastro]: http://openastronomy.org/
[astropy]: https://github.com/astropy/astropy
[proposal]: {{page.previous.url}}
[sofa]: http://www.iausofa.org/
[iau]: http://www.iau.org/
[liberfa]: https://github.com/liberfa/erfa
[erfa_generator]: https://github.com/astropy/astropy/blob/master/astropy/_erfa/erfa_generator.py#L398
