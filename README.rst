=============
vmod_boltsort
=============

-----------------------
Varnish boltsort Module
-----------------------

:Author: Naren Venkataraman
:Date: 2012-09-01
:Version: 1.0
:Manual section: 3

SYNOPSIS
========

At Vimeo we use Varnish Cache to cache our player traffic. It has
worked out great for us.

Varnish at this point has no built in module to sort querystrings.  
Say I have two of these:

* /video/48088296?title=0&byline=0&portrait=0&color=51a516
* /video/48088296?byline=0&color=51a516&portrait=0&title=0

Boltsort sorts the querystring and ensures that Varnish treats the
above one and the same.

DESCRIPTION
===========

The boltsort Varnish vmod sorts querystring.

* Insertion sort on tokenized querystring params makes it twice as fast as current qsort implementations
* Custom param compare instead of storing param lengths for each querystring param means lesser stack usage


FUNCTIONS
=========

sort
-----

Prototype
        ::

                sort(STRING S)
Return value
	STRING
Description
	Returns url with querystring sorted
Example
        ::

                set req.url = boltsort.sort(req.url);

INSTALLATION
============
The source tree is based on autotools to configure the building, and
does also have the necessary bits in place to do functional unit tests
using the varnishtest tool.

Example build instructions:

 ./autogen.sh
 ./configure

Make targets:

* make - builds the vmod
* make install - installs your vmod in `VMODDIR`
* make check - runs the unit tests in ``src/tests/*.vtc``

In your VCL you could then use this vmod along the following lines::
        
        import boltsort;

        sub vcl_hash {
                # sort the querysting and use the sorted one for hashing
                set req.url = boltsort.sort(req.url);
        }

COPYRIGHT
=========

This document is licensed under the same license as the
libvmod-boltsort project. See LICENSE for details.

* Copyright (c) 2012 Vimeo
