---
layout: post
title: Apache™ Subversion®
date: '2018-04-13 10:00'
tags:
  - subversion
  - svn
published: true
categories: IT
---


## Apache™ Subversion®
- Open source version control system
- Full-featured version control system originally designed to be a better CVS


### Vision
Subversion exists to be universally recognized and adopted as an open-source, centralized version control system characterized by its reliability as a safe haven for valuable data; the simplicity of its model and usage; and its ability to support the needs of a wide variety of users and projects, from individuals to large-scale enterprise operations.


### Features
- Most CVS features.
CVS is a relatively basic version control system. For the most part, Subversion has matched or exceeded CVS's feature set where those features continue to apply in Subversion's particular design.

- Directories are versioned.
Subversion versions directories as first-class objects, just like files.

- Copying, deleting, and renaming are versioned.
Copying and deleting are versioned operations. Renaming is also a versioned operation, albeit with some quirks.

- Free-form versioned metadata ("properties").
Subversion allows arbitrary metadata ("properties") to be attached to any file or directory. These properties are key/value pairs, and are versioned just like the objects they are attached to. Subversion also provides a way to attach arbitrary key/value properties to a revision (that is, to a committed changeset). These properties are not versioned, since they attach metadata to the version-space itself, but they can be changed at any time.

- Atomic commits.
No part of a commit takes effect until the entire commit has succeeded. Revision numbers are per-commit, not per-file, and commit's log message is attached to its revision, not stored redundantly in all the files affected by that commit.

- Branching and tagging are cheap (constant time) operations.
There is no reason for these operations to be expensive, so they aren't.

Branches and tags are both implemented in terms of an underlying "copy" operation. A copy takes up a small, constant amount of space. Any copy is a tag; and if you start committing on a copy, then it's a branch as well. (This does away with CVS's "branch-point tagging", by removing the distinction that made branch-point tags necessary in the first place.)

- Merge tracking.
Subversion 1.5 introduces merge tracking: automated assistance with managing the flow of changes between lines of development, and with the merging of branches back into their sources. The 1.5 release of merge tracking has basic support for common scenarios; we will be extending the feature in upcoming releases.

- File locking.
Subversion supports (but does not require) locking files so that users can be warned when multiple people try to edit the same file. A file can be marked as requiring a lock before being edited, in which case Subversion will present the file in read-only mode until a lock is acquired.

- Symbolic links can be versioned.
Unix users can place symbolic links under version control. The links are recreated in Unix working copies, but not in win32 working copies.

- Executable flag is preserved.
Subversion notices when a file is executable, and if that file is placed into version control, its executability will be preserved when it it checked out to other locations. (The mechanism Subversion uses to remember this is simply versioned properties, so executability can be manually edited when necessary, even from a client that does not acknowledge the file's executability, e.g., when having the wrong extension under Microsoft Windows).

- Apache network server option, with WebDAV/DeltaV protocol.
Subversion can use the HTTP-based WebDAV/DeltaV protocol for network communications, and the Apache web server to provide repository-side network service. This gives Subversion an advantage over CVS in interoperability, and allows certain features (such as authentication, wire compression) to be provided in a way that is already familiar to administrators

- Standalone server option (svnserve).
Subversion offers a standalone server option using a custom protocol, since not everyone wants to run an Apache HTTPD server. The standalone server can run as an inetd service or in daemon mode, and offers the same level of authentication and authorization functionality as the HTTPD-based server. The standalone server can also be tunnelled over ssh.

- Parseable output.
All output of the Subversion command-line client is carefully designed to be both human readable and automatically parseable; scriptability is a high priority.

- Localized messages.
Subversion uses gettext() to display translated error, informational, and help messages, based on current locale settings.

- Interactive conflict resolution.
The Subversion command-line client (svn) offers various ways to resolve conflicting changes, include interactive resolution prompting. This mechanism is also made available via APIs, so that other clients (such as graphical clients) can offer interactive conflict resolution appropriate to their interfaces.

- Repository read-only mirroring.
Subversion supplies a utility, svnsync for synchronizing (via either push or pull) a read-only slave repository with a master repository.

- Write-through proxy over WebDAV.
Subversion 1.5 introduces a write-through proxy feature that allows slave repositories (see read-only mirroring) to handle all read operations themselves while passing write operations through to the master. This feature is only available with the Apache HTTPD (WebDAV) server option.

- Natively client/server, layered library design with clean APIs.
Subversion is designed to be client/server from the beginning; thus avoiding some of the maintenance problems which have plagued CVS. The code is structured as a set of modules with well-defined interfaces, designed to be called by other applications.

- Binary files handled efficiently.
Subversion is equally efficient on binary as on text files, because it uses a binary diffing algorithm to transmit and store successive revisions.

- Costs are proportional to change size, not data size.
In general, the time required for a Subversion operation is proportional to the size of the changes resulting from that operation, not to the absolute size of the project in which the changes are taking place.

- Bindings to programming languages.
The Subversion APIs come with bindings for many programming languages, such as Python, Perl, Java, and Ruby. (Subversion itself is written in C.)

- Changelists.
Subversion 1.5 introduces changelists, which allows a user to put modified files into named groups on the client side, and then commit by specifying a particular group. For those who work on logically separate changesets simultaneously in the same directory tree, changelists can help keep things organized.
