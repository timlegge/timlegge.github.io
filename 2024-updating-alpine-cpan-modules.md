# Introduction

Perl packages for alpinelinux are created from APKBUILD files, the same as for other software packages.

At the time of writing Alpine has 354 perl modules in the main repository, 466 in community and 255 number in testing.

# Automatically creating an APKBUILD file for a CPAN module

The [apkbuild-cpan](https://gitlab.alpinelinux.org/alpine/abuild/-/blob/master/apkbuild-cpan.in) script is used to generate APKBUILD files from the CPAN packages.  While the script has existed since at least 2009, it has been improved substantially over the last couple of years to improve the quality of the APKBUILD files created.

The goal of the script is to produce an APKBUILD file of sufficient quality to avoid any manual intervention in the format or the content.  In many cases, the created file can be committed to the repo and submitted without any intervention.

In addition, the script can also upgrade the version of the CPAN module or recreate it from scratch.

# Data

apkbuild-cpan obtains the datarequired to create the APKBUILD file in various ways.

## MetaCPAN API

The metacpan API is queried to provide information about a particular module.

The 'module' and 'distribution' API calls are called to return data that is needed.  This allows the script to avoid knowing anything about the how cpan modules version numbering works by relying on the API to provide that information.

## Dependencies

The generation of dependencies starts with looking for the (MY)META.json and (MY)META.yml files.  If these files do not exist, the script runs the Makefile.PL or the Build.PL file to attempt to generate the META files.

If the metafiles exist, it parses the files for 'requires' and 'runtime' dependencies to generate the APKBUILD files depends.  It uses similar process to generate the makedepends and the checkdepends.

## Issues

There are dependencies in some CPAN packages that are outside the CPAN ecosystem (for example libraries for XS modules).  Currently there is no easy method to derive these dependencies from most CPAN packages.  The apkbuild-cpan script attempts to preserve non-Perl dependencies when it recreates existing APKBUILD files.

