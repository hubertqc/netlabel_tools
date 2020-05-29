Management Tools for the Linux NetLabel Subsystem
==============================================================================
https://github.com/netlabel/netlabel_tools

## Online Resources

The library source repository currently lives on GitHub at the following URL:

* https://github.com/netlabel/netlabel_tools

The project mailing list is currently hosted on Google Groups at the URL below,
please note that a Google account is not required to subscribe to the mailing
list.

* https://groups.google.com/d/forum/netlabel

## Documentation

The "doc/" directory contains all of the currently available documentation,
mostly in the form of manpages.  The top level directory also contains a README
file (this file) as well as the LICENSE, SUBMITTING_PATCHES, and CHANGELOG
files.

Those who are interested in contributing to the the project are encouraged to
read the SUBMITTING_PATCHES in the top level directory.

## Building and Installing

If you are building the NetLabel tools package from an official release
tarball, you should follow the familiar three step process used by most
autotools based applications:

	% ./configure
	% make [V=0|1]
	% make install

However, if you are building the library from sources retrieved from the source
repository you may need to run the autogen.sh script before running configure.
In both cases, running "./configure -h" will display a list of build-time
configuration options.

## NetLabel Configuration Quick Start

This section assumes you are already running a kernel with NetLabel support,
if you are not please configure your kernel for NetLabel support before going
any further.  Once you have unpacked the NetLabel tools tarball and built the
netlabelctl management application as described above, you can proceed with
the following configuration steps.

If you are unsure about the necessary kernel support, or even the current
NetLabel configuration, you can both verify the kernel and display the current
configuration with the following commands:

	% netlabelctl -p cipso list
	% netlabelctl -p map list

If you see any configured CIPSO definitions you can remove them with the
following command:

	% netlabelctl -p cipso del doi:<DOI>

If you see any domain mappings you can remove them with the following command:

	% netlabelctl -p map del domain:<DOMAIN>

You can remove the default domain mapping with the command below, although
you should proceed with caution as outbound traffic without an associated
mapping is dropped.

	% netlabelctl -p map del default

Finally, you set NetLabel to allow or deny incoming unlabeled packets with
the following command:

	% netlabelctl -p unlbl accept on|off

Now that you have removed any existing NetLabel configuration you can setup a
basic CIPSO configuration.  The first step is to add a CIPSO/IPv4 definition
to the kernel.  The command below creates a CIPSO/IPv4 definition using a DOI
value of 1, the permissive bitmask tag (value 1), and a pass through mapping
meaning the CIPSO MLS values are passed straight through to the LSM.

	% netlabelctl cipso add pass doi:1 tags:1

The next step is to tell the NetLabel system to use this CIPSO/IPv4 defintion
by default.  You do that with the following command:

	% netlabelctl map add default protocol:cipso,1

You can verify that everything is configured correctly with the following two
commands:

	% netlabelctl -p cipso list doi:1
	% netlabelctl -p map list

For a more in depth explanation of configuring NetLabel on your Linux system,
please see the information in the "doc/" directory.
