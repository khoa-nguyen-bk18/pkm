---
title: "Running qpdf — qpdf 12.0.0 documentation"
source: "https://qpdf.readthedocs.io/en/stable/cli.html"
author:
published:
created: 2025-03-27
description:
tags:
  - "clippings"
---
## Running qpdf

This chapter describes how to run the qpdf program from the command line.

## Basic Invocation

```
Usage: qpdf [infile] [options] [outfile]
```

The **qpdf** command reads the PDF file `*infile*`, applies various transformations or modifications to the file in memory, and writes the result to `*outfile*`. When run with no options, the output file is functionally identical to the input file but may be structurally reorganized, and orphaned objects are removed from the file. Many options are available for applying transformations or modifications to the file.

`*infile*` can be a regular file, or it can be to start with an empty PDF file. There is no way to use standard input since the input file has to be seekable.

`*outfile*` can be a regular file, `-` to represent standard output, or to indicate that the input file should be overwritten. The output file does not have to be seekable, even when generating linearized files. You can also use to create separate output files for each page (or group of pages) instead of a single output file.

Password-protected files may be opened by specifying a password with.

All options other than help options (see ) require an input file. If inspection or JSON options (see and ) or help options are given, an output file must not be given. Otherwise, an output file is required.

If `@filename` appears as a word anywhere in the command-line, it will be read line by line, and each line will be treated as a command-line argument. Leading and trailing whitespace is intentionally not removed from lines, which makes it possible to handle arguments that start or end with spaces. The `@-` option allows arguments to be read from standard input. This allows qpdf to be invoked with an arbitrary number of arbitrarily long arguments. It is also very useful for avoiding having to pass passwords on the command line, though see also . Note that the `@filename` can’t appear in the middle of an argument, so constructs such as `--arg=@filename` will not work. Instead, you would have to include the option and its parameter (e.g.,`--option=parameter` ) as a line in the `filename` file and just pass `@filename` on the command line.

## Exit Status

The exit status of **qpdf** may be interpreted as follows:

| 0 | no errors or warnings were found |
| --- | --- |
| 1 | not used |
| 2 | errors were found; the file was not processed |
| 3 | warnings were found without errors |

Notes:

- A PDF file may have problems that qpdf can’t detect.
- With the option, exit status `0` is used even if there are warnings.
- **qpdf** does not exit with status `1` since the shell uses this exit code if it is unable to invoke the command.
- If both errors and warnings were found, exit status `2` is used.
- The and options use different exit codes. See their help for details.

### Related Options

\--warning-exit-0 [](https://qpdf.readthedocs.io/en/stable/#option-warning-exit-0 "Link to this definition")

If there were warnings only and no errors, exit with exit code `0` instead of `3`. When combined with , the effect is for **qpdf** to completely ignore warnings.

## Shell Completion

**qpdf** provides its own completion support for zsh and bash. You can enable bash completion with **eval $(qpdf --completion-bash)** and zsh completion with **eval $(qpdf --completion-zsh)**. If **qpdf** is not in your path, you should use an absolute path to qpdf in the above invocation. If you invoke it with a relative path, it will warn you, and the completion won’t work if you’re in a different directory.

**qpdf** will use `argv[0]` to figure out where its executable is. This may produce unwanted results in some cases, especially if you are trying to use completion with a copy of qpdf that is run directly out of the source tree or that is invoked with a wrapper script. You can specify a full path to the qpdf you want to use for completion in the `QPDF_EXECUTABLE` environment variable.

### Related Options

\--completion-bash [](https://qpdf.readthedocs.io/en/stable/#option-completion-bash "Link to this definition")

Output a completion command you can eval to enable shell completion from bash.

\--completion-zsh [](https://qpdf.readthedocs.io/en/stable/#option-completion-zsh "Link to this definition")

Output a completion command you can eval to enable shell completion from zsh.

## Help/Information

Help options provide some information about qpdf itself. Help options are only valid as the first and only command-line argument.

### Related Options

\--help\[=--option|topic\] [](https://qpdf.readthedocs.io/en/stable/#option-help "Link to this definition")

Display command-line invocation help. Use `--help=*--option*` for help on a specific option and `--help=*topic*` for help on a help topic and also provides a list of available help topics.

\--version [](https://qpdf.readthedocs.io/en/stable/#option-version "Link to this definition")

Display the version of qpdf. The version number displayed is the one that is compiled into the qpdf library. If you don’t see the version number you expect, you may have more than one version of **qpdf** installed and may not have your library path set up correctly.

\--copyright [](https://qpdf.readthedocs.io/en/stable/#option-copyright "Link to this definition")

Display copyright and license information.

\--show-crypto [](https://qpdf.readthedocs.io/en/stable/#option-show-crypto "Link to this definition")

Show a list of available crypto providers, each on a line by itself. The default provider is always listed first. See [Crypto Providers](https://qpdf.readthedocs.io/en/stable/installation.html#crypto) for more information about crypto providers.

\--job-json-help [](https://qpdf.readthedocs.io/en/stable/#option-job-json-help "Link to this definition")

Describe the format of the QPDFJob JSON input used by. For more information about QPDFJob, see [QPDFJob: a Job-Based Interface](https://qpdf.readthedocs.io/en/stable/qpdf-job.html#qpdf-job).

\--zopfli [](https://qpdf.readthedocs.io/en/stable/#option-zopfli "Link to this definition")

If zopfli support is compiled in, indicate whether it is active, and exit normally. Otherwise, indicate that it is not compiled in, and exit with an error code. If zopfli is compiled in, activate it by setting the `QPDF_ZOPFLI` environment variable. See.

## General Options

This section describes general options that control **qpdf** ’s behavior. They are not necessarily related to the specific operation that is being performed and may be used whether or not an output file is being created.

### Related Options

\--password=password [](https://qpdf.readthedocs.io/en/stable/#option-password "Link to this definition")

Specifies a password for accessing encrypted, password-protected files. To read the password from a file or standard input, you can use .

Prior to 8.4.0, in the case of passwords that contain characters that fall outside of 7-bit US-ASCII, qpdf left the burden of supplying properly encoded encryption and decryption passwords to the user. Starting in qpdf 8.4.0, qpdf does this automatically in most cases. For an in-depth discussion, please see. Previous versions of this manual described workarounds using the **iconv** command. Such workarounds are no longer required or recommended starting with qpdf 8.4.0. However, for backward compatibility, qpdf attempts to detect those workarounds and do the right thing in most cases.

\--password-file=filename [](https://qpdf.readthedocs.io/en/stable/#option-password-file "Link to this definition")

Reads the first line from the specified file and uses it as the password for accessing encrypted files. `*filename*` may be `-` to read the password from standard input, but if you do that the password is echoed and there is no prompt, so use `-` with caution. Note that leading and trailing spaces are not stripped from the password.

\--verbose [](https://qpdf.readthedocs.io/en/stable/#option-verbose "Link to this definition")

Increase verbosity of output. This includes information about files created, image optimization, and several other operations. In some cases, it also displays additional information when inspection options (see ) are used.

\--progress [](https://qpdf.readthedocs.io/en/stable/#option-progress "Link to this definition")

Indicate progress while writing output files. Progress indication does not start until writing starts, so there may be a delay before progress indicators are seen if complicated transformations are being applied before the write process begins.

\--no-warn [](https://qpdf.readthedocs.io/en/stable/#option-no-warn "Link to this definition")

Suppress writing of warnings to stderr. If warnings were detected and suppressed, **qpdf** will still exit with exit code 3. To completely ignore warnings, also specify. Use with caution as qpdf is not always successful in recovering from situations that cause warnings to be issued.

\--deterministic-id [](https://qpdf.readthedocs.io/en/stable/#option-deterministic-id "Link to this definition")

Generate a secure, random document ID using deterministic values. This prevents use of timestamp and output file name information in the ID generation. Instead, at some slight additional runtime cost, the ID field is generated to include a digest of the significant parts of the content of the output PDF file. This means that a given qpdf operation should generate the same ID each time it is run, which can be useful when caching results or for generation of some test data. Use of this flag is not compatible with creation of encrypted files. This option can be useful for testing. See also.

While qpdf will generate the same deterministic ID given the same output PDF, there is no guarantee that different versions of qpdf will generate exactly the same PDF output for the same input and options. While care is taken to avoid gratuitous changes to qpdf’s PDF generation, new versions of qpdf may include changes or bug fixes that cause slightly different PDF code to be generated. Such changes are noted in the release notes.

\--allow-weak-crypto [](https://qpdf.readthedocs.io/en/stable/#option-allow-weak-crypto "Link to this definition")

Encrypted PDF files using 40-bit keys or 128-bit keys without AES use the insecure *RC4* encryption algorithm. Starting with version 11.0, qpdf’s default behavior is to refuse to write files using RC4 encryption. Use this option to allow creation of such files. In versions 10.4 through 10.6, attempting to create weak encrypted files was a warning, rather than an error, without this flag. See [Weak Cryptography](https://qpdf.readthedocs.io/en/stable/weak-crypto.html#weak-crypto) for additional details.

No check is performed for weak crypto when preserving encryption parameters from or copying encryption parameters from other files. The rationale for this is discussed in [Weak Cryptography](https://qpdf.readthedocs.io/en/stable/weak-crypto.html#weak-crypto).

\--keep-files-open=\[y|n\] [](https://qpdf.readthedocs.io/en/stable/#option-keep-files-open "Link to this definition")

This option controls whether qpdf keeps individual files open while merging. By default, qpdf keeps files open when merging unless more than 200 files are specified, in which case files are opened as needed and closed when finished. Repeatedly opening and closing files may impose a large performance penalty with some file systems, especially networked file systems. If you know that you have a large enough open file limit and are suffering from performance problems, or if you have an open file limit smaller than 200, you can use this option to override the default behavior by specifying `--keep-files-open=y` to force **qpdf** to keep files open or `--keep-files-open=n` to force it to only open files as needed. See also.

Historical note: prior to version 8.1.0, qpdf always kept all files open, but this meant that the number of files that could be merged was limited by the operating system’s open file limit. Version 8.1.0 opened files as they were referenced and closed them after each read, but this caused a major performance impact. Version 8.2.0 optimized the performance but did so in a way that, for local file systems, there was a small but unavoidable performance hit, but for networked file systems the performance impact could be very high. The current behavior was introduced in qpdf version 8.2.1.

\--keep-files-open-threshold=count [](https://qpdf.readthedocs.io/en/stable/#option-keep-files-open-threshold "Link to this definition")

If specified, overrides the default value of 200 used as the threshold for qpdf deciding whether or not to keep files open. See for details.

## Advanced Control Options

Advanced control options control qpdf’s behavior in ways that would normally never be needed by a user but that may be useful to developers or people investigating problems with specific files.

### Related Options

\--password-is-hex-key [](https://qpdf.readthedocs.io/en/stable/#option-password-is-hex-key "Link to this definition")

Overrides the usual computation/retrieval of the PDF file’s encryption key from user/owner password with an explicit specification of the encryption key. When this option is specified, the parameter to the option is interpreted as a hexadecimal-encoded key value. This only applies to the password used to open the main input file. It does not apply to other files opened by or other options or to files being written.

Most users will never have a need for this option, and no standard viewers support this mode of operation, but it can be useful for forensic or investigatory purposes. For example, if a PDF file is encrypted with an unknown password, a brute-force attack using the key directly is sometimes more efficient than one using the password. Also, if a file is heavily damaged, it may be possible to derive the encryption key and recover parts of the file using it directly. To expose the encryption key used by an encrypted file that you can open normally, use the option.

\--suppress-password-recovery [](https://qpdf.readthedocs.io/en/stable/#option-suppress-password-recovery "Link to this definition")

Ordinarily, qpdf attempts to automatically compensate for passwords encoded with the wrong character encoding. This option suppresses that behavior. Under normal conditions, there are no reasons to use this option. See for a discussion.

\--password-mode=mode [](https://qpdf.readthedocs.io/en/stable/#option-password-mode "Link to this definition")

This option can be used to fine-tune how qpdf interprets Unicode (non-ASCII) password strings passed on the command line. With the exception of the `hex-bytes` mode, these only apply to passwords provided when encrypting files. The `hex-bytes` mode also applies to passwords specified for reading files. For additional discussion of the supported password modes and when you might want to use them, see . The following modes are supported:

- `auto`: Automatically determine whether the specified password is a properly encoded Unicode (UTF-8) string, and transcode it as required by the PDF spec based on the type of encryption being applied. On Windows starting with version 8.4.0, and on almost all other modern platforms, incoming passwords will be properly encoded in UTF-8, so this is almost always what you want.
- `unicode`: Tells qpdf that the incoming password is UTF-8, overriding whatever its automatic detection determines. The only difference between this mode and `auto` is that qpdf will fail with an error message if the password is not valid UTF-8 instead of falling back to `bytes` mode with a warning.
- `bytes`: Interpret the password as a literal byte string. For non-Windows platforms, this is what versions of qpdf prior to 8.4.0 did. For Windows platforms, there is no way to specify strings of binary data on the command line directly, but you can use a `@filename` option or to do it, in which case this option forces qpdf to respect the string of bytes as provided. Note that this option may cause you to encrypt PDF files with passwords that will not be usable by other readers.
- `hex-bytes`: Interpret the password as a hex-encoded string. This provides a way to pass binary data as a password on all platforms including Windows. As with `bytes`, this option may allow creation of files that can’t be opened by other readers. This mode affects qpdf’s interpretation of passwords specified for decrypting files as well as for encrypting them. It makes it possible to specify strings that are encoded in some manner other than the system’s default encoding.

\--suppress-recovery [](https://qpdf.readthedocs.io/en/stable/#option-suppress-recovery "Link to this definition")

Prevents qpdf from attempting to reconstruct a file’s cross reference table when there are errors reading objects from the file. Recovery is triggered by a variety of situations. While usually successful, it uses heuristics that don’t work on all files. If this option is given, **qpdf** fails on the first error it encounters.

\--ignore-xref-streams [](https://qpdf.readthedocs.io/en/stable/#option-ignore-xref-streams "Link to this definition")

Tells qpdf to ignore any cross-reference streams, falling back to any embedded cross-reference tables or triggering document recovery. Ordinarily, qpdf reads cross-reference streams when they are present in a PDF file. If this option is specified, qpdf will ignore any cross-reference streams for hybrid PDF files. The purpose of hybrid files is to make some content available to viewers that are not aware of cross-reference streams. It is almost never desirable to ignore them. The only time when you might want to use this feature is if you are testing creation of hybrid PDF files and wish to see how a PDF consumer that doesn’t understand object and cross-reference streams would interpret such a file.

## PDF Transformation

The options discussed in this section tell qpdf to apply transformations that change the structure of a PDF file without changing its content. Examples include creating linearized (web-optimized) files, adding or removing encryption, restructuring files for older viewers, and rewriting files for human inspection. See also .

### Related Options

\--linearize [](https://qpdf.readthedocs.io/en/stable/#option-linearize "Link to this definition")

Create linearized (web-optimized) output files. Linearized files are formatted in a way that allows compliant readers to begin displaying a PDF file before it is fully downloaded. Ordinarily, the entire file must be present before it can be rendered because important cross-reference information typically appears at the end of the file.

\--encrypt \[options\] \-- [](https://qpdf.readthedocs.io/en/stable/#option-encrypt "Link to this definition")

This flag starts encryption options, used to create encrypted files. Please see for details.

\--decrypt [](https://qpdf.readthedocs.io/en/stable/#option-decrypt "Link to this definition")

Create an output file with no encryption even if the input file is encrypted. This option overrides the default behavior of preserving whatever encryption was present on the input file. This functionality is not intended to be used for bypassing copyright restrictions or other restrictions placed on files by their producers. See also and.

\--remove-restrictions [](https://qpdf.readthedocs.io/en/stable/#option-remove-restrictions "Link to this definition")

Remove security restrictions associated with digitally signed PDF files. This may be combined with to allow free editing of previously signed/encrypted files. This option invalidates and disables any digital signatures but leaves their visual appearances intact.

\--copy-encryption=file [](https://qpdf.readthedocs.io/en/stable/#option-copy-encryption "Link to this definition")

Copy all encryption parameters, including the user password, the owner password, and all security restrictions, from the specified file instead of preserving the encryption details from the input file. This works even if only one of the user password or owner password is known. If the encryption file requires a password, use the option to set it. Note that copying the encryption parameters from a file also copies the first half of `/ID` from the file since this is part of the encryption parameters. This option can be useful if you need to decrypt a file to make manual changes to it or to change it outside of qpdf, and then want to restore the original encryption on the file without having to manual specify all the individual settings. See also .

Checks for weak cryptographic algorithms are intentionally not made by this operation. See [Weak Cryptography](https://qpdf.readthedocs.io/en/stable/weak-crypto.html#weak-crypto) for the rationale.

\--encryption-file-password=password [](https://qpdf.readthedocs.io/en/stable/#option-encryption-file-password "Link to this definition")

If the file specified with requires a password, supply the password using this option. This option is necessary because the option applies to the input file, not the file from which encryption is being copied.

\--qdf [](https://qpdf.readthedocs.io/en/stable/#option-qdf "Link to this definition")

Create a PDF file suitable for viewing and editing in a text editor. This is to edit the PDF code, not the page contents. To edit a QDF file, your text editor must preserve binary data. In a QDF file, all streams that can be uncompressed are uncompressed, and content streams are normalized, among other changes. The companion tool **fix-qdf** can be used to repair hand-edited QDF files. QDF is a feature specific to the qpdf tool. For additional information, see [QDF Mode](https://qpdf.readthedocs.io/en/stable/qdf.html#qdf). Note that disables QDF mode.

QDF mode has full support for object streams, but sometimes it’s easier to locate a specific object if object streams are disabled. When trying to understand some PDF construct by inspecting an existing file, it can be useful to combine `--qdf` with `--object-streams=disable`.

This flag changes some of the defaults of other options: stream data is uncompressed, content streams are normalized, and encryption is removed. These defaults can still be overridden by specifying the appropriate options with `--qdf`. Additionally, in QDF mode, stream lengths are stored as indirect objects, objects are formatted in a less efficient but more readable fashion, and the documents are interspersed with comments that make it easier for the user to find things and also make it possible for **fix-qdf** to work properly. When editing QDF files, it is not necessary to maintain the object formatting.

When normalizing content, if qpdf runs into any lexical errors, it will print a warning indicating that content may be damaged. If you want to create QDF files without content normalization, specify `--qdf --normalize-content=n`. You can also create a non-QDF file with uncompressed streams using `--stream-data=uncompress`. Either option will uncompress all the streams but will not attempt to normalize content. Please note that if you are using content normalization or QDF mode for the purpose of manually inspecting files, you don’t have to care about this.

See also .

\--no-original-object-ids [](https://qpdf.readthedocs.io/en/stable/#option-no-original-object-ids "Link to this definition")

Suppresses inclusion of original object ID comments in QDF files. This can be useful when generating QDF files for test purposes, particularly when comparing them to determine whether two PDF files have identical content. The original object ID comment is there by default because it makes it easier to trace objects back to the original file.

\--compress-streams=\[y|n\] [](https://qpdf.readthedocs.io/en/stable/#option-compress-streams "Link to this definition")

By default, or with `--compress-streams=y`, qpdf will compress streams using the flate compression algorithm (used by zip and gzip) unless those streams are compressed in some other way. This analysis is made after qpdf attempts to uncompress streams and is therefore closely related to . To suppress this behavior and leave streams uncompressed, use `--compress-streams=n`. In QDF mode (see [QDF Mode](https://qpdf.readthedocs.io/en/stable/qdf.html#qdf) and), the default is to leave streams uncompressed.

\--decode-level=parameter [](https://qpdf.readthedocs.io/en/stable/#option-decode-level "Link to this definition")

Controls which streams qpdf tries to decode. The default is `generalized`.

The following values for `*parameter*` are available:

- `none`: do not attempt to decode any streams. This is the default with .
- `generalized`: decode streams filtered with supported generalized filters: `/LZWDecode`, `/FlateDecode`,`/ASCII85Decode`, and `/ASCIIHexDecode`. We define generalized filters as those to be used for general-purpose compression or encoding, as opposed to filters specifically designed for image data. This is the default except when is given.
- `specialized`: in addition to generalized, decode streams with supported non-lossy specialized filters; currently this is just `/RunLengthDecode`
- `all`: in addition to generalized and specialized, decode streams with supported lossy filters; currently this is just `/DCTDecode` (JPEG)

There are several filters that **qpdf** does not support. These are left untouched regardless of the option. Future versions of qpdf may support additional filters.

Because the default value is `generalized`, qpdf’s default behavior is to uncompress any stream that is encoded using non-lossy filters that qpdf understands. If `--compress-streams=y` is also in effect, which is the default (see ), the overall effect is that qpdf will recompress streams with generalized filters using flate compression, effectively eliminating LZW and ASCII-based filters. This is usually desirable behavior but can be disabled with `--decode-level=none`. Note that `--decode-level=none` is the default when is specified, but it can be overridden in that case as well.

As a special case, streams already compressed with `/FlateDecode` are not uncompressed and recompressed. You can change this behavior with . See also .

\--stream-data=parameter [](https://qpdf.readthedocs.io/en/stable/#option-stream-data "Link to this definition")

Controls transformation of stream data. This option predates the and options. Those options can be used to achieve the same effect with more control. The value of `*parameter*` may be one of the following:

- `compress`: recompress stream data when possible (default); equivalent to `--compress-streams=y` `--decode-level=generalized`. Does not recompress streams already compressed with `/FlateDecode` unless is also specified.
- `preserve`: leave all stream data as is; equivalent to `--compress-streams=n` `--decode-level=none`
- `uncompress`: uncompress stream data compressed with generalized filters when possible; equivalent to `--compress-streams=n` `--decode-level=generalized`

\--recompress-flate [](https://qpdf.readthedocs.io/en/stable/#option-recompress-flate "Link to this definition")

The default generalized compression scheme used by PDF is flate ( `/FlateDecode` ), which is the same as used by **zip** and **gzip**. Usually qpdf just leaves these alone. This option tells **qpdf** to uncompress and recompress streams compressed with flate. This can be useful when combined with. Using this option may make **qpdf** much slower when writing output files. See also.

\--compression-level=level [](https://qpdf.readthedocs.io/en/stable/#option-compression-level "Link to this definition")

When writing new streams that are compressed with `/FlateDecode`, use the specified compression level. The value of `level` should be a number from 1 to 9 and is passed directly to zlib, which implements deflate compression. Lower numbers compress less and are faster; higher numbers compress more and are slower. Note that **qpdf** doesn’t uncompress and recompress streams compressed with flate by default. To have this option apply to already compressed streams, you should also specify. If your goal is to shrink the size of PDF files, you should also use `--object-streams=generate`. If you omit this option, qpdf defers to the compression library’s default behavior. See also.

\--normalize-content=\[y|n\] [](https://qpdf.readthedocs.io/en/stable/#option-normalize-content "Link to this definition")

Enables or disables normalization of newlines in PDF content streams to UNIX-style newlines, which is useful for viewing files in a programmer-friendly text edit across multiple platforms. Content normalization is off by default, but is automatically enabled by (see also [QDF Mode](https://qpdf.readthedocs.io/en/stable/qdf.html#qdf) ). It is not recommended to use this option for production use. If qpdf runs into any lexical errors while normalizing content, it will print a warning indicating that content may be damaged.

\--object-streams=mode [](https://qpdf.readthedocs.io/en/stable/#option-object-streams "Link to this definition")

Controls handling of object streams. The value of `*mode*` may be one of the following:

| `preserve` | preserve original object streams, if any (the default) |
| --- | --- |
| `disable` | create output files with no object streams |
| `generate` | create object streams, and compress objects when possible |

Object streams are PDF streams that contain other objects. Putting objects into object streams allows the PDF objects themselves to be compressed, which can result in much smaller PDF files. Combining this option with and can often result in the creation of smaller PDF files.

Object streams, also known as compressed objects, were introduced into the PDF specification at version 1.5 around 2003. Some ancient PDF viewers may not support files with object streams. qpdf can be used to transform files with object streams into files without object streams or vice versa.

In `preserve` mode, the relationship between objects and the streams that contain them is preserved from the original file. If the file has no object streams, qpdf will not add any. In `disable` mode, all objects are written as regular, uncompressed objects. The resulting file should be structurally readable by older PDF viewers, though there is still a chance that the file may contain other content that some older readers can’t support. In `generate` mode, qpdf will create its own object streams. This will usually result in more compact PDF files. In this mode, qpdf will also make sure the PDF version number in the header is at least 1.5.

\--preserve-unreferenced [](https://qpdf.readthedocs.io/en/stable/#option-preserve-unreferenced "Link to this definition")

Tells qpdf to preserve objects that are not referenced when writing the file. Ordinarily any object that is not referenced in a traversal of the document from the trailer dictionary will be discarded. Disabling this default behavior may be useful in working with some damaged files or inspecting files with known unreferenced objects.

This flag is ignored for linearized files and has the effect of causing objects in the new file to be written ordered by object ID from the original file. This does not mean that object numbers will be the same since qpdf may create stream lengths as direct or indirect differently from the original file, and the original file may have gaps in its numbering.

See also , which does something completely different.

\--remove-unreferenced-resources=parameter [](https://qpdf.readthedocs.io/en/stable/#option-remove-unreferenced-resources "Link to this definition")

Parameters: `auto` (the default), `yes`, or `no`.

Starting with qpdf 8.1, when splitting pages, qpdf is able to attempt to remove images and fonts that are not used by a page even if they are referenced in the page’s resources dictionary. When shared resources are in use, this behavior can greatly reduce the file sizes of split pages, but the analysis is very slow. In versions from 8.1 through 9.1.1, qpdf did this analysis by default. Starting in qpdf 10.0.0, if `auto` is used, qpdf does a quick analysis of the file to determine whether the file is likely to have unreferenced objects on pages, a pattern that frequently occurs when resource dictionaries are shared across multiple pages and rarely occurs otherwise. If it discovers this pattern, then it will attempt to remove unreferenced resources. Usually this means you get the slower splitting speed only when it’s actually going to create smaller files. You can suppress removal of unreferenced resources altogether by specifying `no` or force qpdf to do the full algorithm by specifying `yes`.

Other than cases in which you don’t care about file size and care a lot about runtime, there are few reasons to use this option, especially now that `auto` mode is supported. One reason to use this is if you suspect that qpdf is removing resources it shouldn’t be removing. If you encounter such a case, please report it as a bug at [https://github.com/qpdf/qpdf/issues/](https://github.com/qpdf/qpdf/issues/).

\--preserve-unreferenced-resources [](https://qpdf.readthedocs.io/en/stable/#option-preserve-unreferenced-resources "Link to this definition")

This is a synonym for `--remove-unreferenced-resources=no`. See .

See also , which does something completely different. To reduce confusion, you should use `--remove-unreferenced-resources=no` instead.

\--newline-before-endstream [](https://qpdf.readthedocs.io/en/stable/#option-newline-before-endstream "Link to this definition")

Tell qpdf to insert a newline before the `endstream` keyword, not counted in the length, after any stream content even if the last character of the stream was a newline. This may result in two newlines in some cases. This is a requirement of PDF/A. While qpdf doesn’t specifically know how to generate PDF/A-compliant PDFs, this at least prevents it from removing compliance on already compliant files.

\--coalesce-contents [](https://qpdf.readthedocs.io/en/stable/#option-coalesce-contents "Link to this definition")

When a page’s contents are split across multiple streams, this option causes qpdf to combine them into a single stream. Use of this option is never necessary for ordinary usage, but it can help when working with some files in some cases. For example, this can be combined with QDF mode or content normalization to make it easier to look at all of a page’s contents at once. It is common for PDF writers to create multiple content streams for a variety of reasons such as making it easier to modify page contents and splitting very large content streams so PDF viewers may be able to use less memory.

\--externalize-inline-images [](https://qpdf.readthedocs.io/en/stable/#option-externalize-inline-images "Link to this definition")

Convert inline images to regular images. By default, images whose data is at least 1,024 bytes are converted when this option is selected. Use to change the size threshold. This option is implicitly selected when is selected unless is also specified.

\--ii-min-bytes=size-in-bytes [](https://qpdf.readthedocs.io/en/stable/#option-ii-min-bytes "Link to this definition")

Avoid converting inline images whose size is below the specified minimum size to regular images. The default is 1,024 bytes. Use 0 for no minimum.

\--min-version=version [](https://qpdf.readthedocs.io/en/stable/#option-min-version "Link to this definition")

Force the PDF version of the output file to be at least `*version*`. In other words, if the input file has a lower version than the specified version, the specified version will be used. If the input file has a higher version, the input file’s original version will be used. It is seldom necessary to use this option since qpdf will automatically increase the version as needed when adding features that require newer PDF readers.

The version number may be expressed in the form `*major*.*minor*[.*extension-level*]`. If`.*extension-level*`, is given, version is interpreted as `*major.minor*` at extension level `*extension-level*`. For example, version `1.7.8` represents version 1.7 at extension level 8. Note that minimal syntax checking is done on the command line. **qpdf** does not check whether the specified version is actually required.

\--force-version=version [](https://qpdf.readthedocs.io/en/stable/#option-force-version "Link to this definition")

This option forces the PDF version to be the exact version specified *even when the file may have content that is not supported in that version*. The version number is interpreted in the same way as with so that extension levels can be set. In some cases, forcing the output file’s PDF version to be lower than that of the input file will cause qpdf to disable certain features of the document. Specifically, 256-bit keys are disabled if the version is less than 1.7 with extension level 8 (except the deprecated, unsupported “R5” format is allowed with extension levels 3 through 7), AES encryption is disabled if the version is less than 1.6, cleartext metadata and object streams are disabled if less than 1.5, 128-bit encryption keys are disabled if less than 1.4, and all encryption is disabled if less than 1.3. Even with these precautions, qpdf won’t be able to do things like eliminate use of newer image compression schemes, transparency groups, or other features that may have been added in more recent versions of PDF.

As a general rule, with the exception of big structural things like the use of object streams or AES encryption, PDF viewers are supposed to ignore features they don’t support. This means that forcing the version to a lower version may make it possible to open your PDF file with an older version, though bear in mind that some of the original document’s functionality may be lost.

## Page Ranges

Several **qpdf** command-line options use page ranges. This section describes the syntax of a page range.

- A plain number indicates a page numbered from `1`, so `1` represents the first page.
- A number preceded by `r` counts from the end, so `r1` is the last page, `r2` is the second-to-last page, etc.
- The letter `z` represents the last page and is the same as `r1`.
- Page numbers may appear in any order separated by commas.
- Two page numbers separated by dashes represents the inclusive range of pages from the first to the second. If the first number is higher than the second number, it is the range of pages in reverse.
- A number or dash-separated range of numbers may be prepended with `x` (from qpdf 11.7.1). This means to exclude the pages in that range from the previous range that didn’t start with `x`.
- The range may be appended with `:odd` or `:even` to select only pages from the resulting range in odd or even positions. In this case, odd and even refer to positions in the final range, not whether the original page number is odd or even.

| `1,6,4` | pages 1, 6, and 4 in that order |
| --- | --- |
| `3-7` | pages 3 through 7 inclusive in increasing order |
| `7-3` | pages 7, 6, 5, 4, and 3 in that order |
| `1-z` | all pages in order |
| `z-1` | all pages in reverse order |
| `1,3,5-9,15-12` | pages 1, 3, 5, 6, 7, 8, 9, 15, 14, 13, and 12 in that order |
| `r3-r1` | the last three pages of the document |
| `r1-r3` | the last three pages of the document in reverse order |
| `1-20:even` | even pages from 2 to 20 |
| `5,7-9,12` | pages 5, 7, 8, 9, and 12 |
| `5,7-9,12:odd` | pages 5, 8, and 12, which are the pages in odd positions from the original set of 5, 7, 8, 9, 12 |
| `5,7-9,12:even` | pages 7 and 9, which are the pages in even positions from the original set of 5, 7, 8, 9, 12 |
| `1-10,x3-4` | pages 1 through 10 except pages 3 and 4 (1, 2, and 5 through 10) |
| `4-10,x7-9,12-8,xr5` | In a 15-page file, this is 4, 5, 6, 10, 12, 10, 9, and 8 in that order. That is pages 4 through 10 except 7 through 9 followed by 12 through 8 descending except 11 (the fifth page from the end) |

## PDF Modification

Modification options make systematic changes to certain parts of the PDF, causing the PDF to render differently from the original. See also.

### Related Options

\--pages \[--file=\]file \[options\] \[...\] \-- [](https://qpdf.readthedocs.io/en/stable/#option-pages "Link to this definition")

This flag starts page selection options, which are used to select pages from one or more input files to perform operations such as splitting, merging, and collating files.

Please see for details about selecting pages.

See also , ,.

\--file=file [](https://qpdf.readthedocs.io/en/stable/#option-file "Link to this definition")

Specify the file for the current page operation. This option is used with , and and appears between the option and the terminating `--`.

Please see for additional details.

\--range=numeric-range [](https://qpdf.readthedocs.io/en/stable/#option-range "Link to this definition")

Specify the page range for the current page operation with. If omitted, all pages are selected. This option is used with and appears between and `--`.

Please see for additional details.

\--collate\[=n\[,m,...\]\] [](https://qpdf.readthedocs.io/en/stable/#option-collate "Link to this definition")

This option causes **qpdf** to collate rather than concatenate pages specified with . With a numeric parameter, collate in groups of `*n*`. The default is 1. With comma-separated numeric parameters, take `*n*` from the first file, `*m*` from the second, etc.

Please see for additional details.

\--split-pages\[=n\] [](https://qpdf.readthedocs.io/en/stable/#option-split-pages "Link to this definition")

Write each group of `*n*` pages to a separate output file. If `*n*` is not specified, create single pages. Output file names are generated as follows:

- If the string `%d` appears in the output file name, it is replaced with a range of zero-padded page numbers starting from 1.
- Otherwise, if the output file name ends in `.pdf` (case insensitive), a zero-padded page range, preceded by a dash, is inserted before the file extension.
- Otherwise, the file name is appended with a zero-padded page range preceded by a dash.

Zero padding is added to all page numbers in file names so that all the numbers are the same length, which causes the output filenames to sort lexically in numerical order.

Page ranges are a single number in the case of single-page groups or two numbers separated by a dash otherwise.

Here are some examples. In these examples, `infile.pdf` has 12 pages.

- `qpdf --split-pages infile.pdf %d-out`: output files are `01-out` through `12-out` with no extension.
- `qpdf --split-pages=2 infile.pdf outfile.pdf`: output files are `outfile-01-02.pdf` through `outfile-11-12.pdf`
- `qpdf --split-pages infile.pdf something.else` would generate files `something.else-01` through `something.else-12`. The extension `.else` is not treated in any special way regarding the placement of the number.

Note that outlines, threads, and other document-level features of the original PDF file are not preserved. For each page of output, this option creates an empty PDF and copies a single page from the output into it. If you require the document-level data, you will have to run **qpdf** with the option once for each page. Using is much faster if you don’t require the document-level data. A future version of qpdf may support preservation of some document-level information.

Overlay pages from another PDF file on the output.

See for details.

\--underlay file \[options\] \-- [](https://qpdf.readthedocs.io/en/stable/#option-underlay "Link to this definition")

Underlay pages from another PDF file on the output.

See for details.

\--flatten-rotation [](https://qpdf.readthedocs.io/en/stable/#option-flatten-rotation "Link to this definition")

For each page that is rotated using the `/Rotate` key in the page’s dictionary, remove the `/Rotate` key and implement the identical rotation semantics by modifying the page’s contents. This option can be useful to prepare files for buggy PDF applications that don’t properly handle rotated pages. There is usually no reason to use this option unless you are working around a specific problem.

\--flatten-annotations=parameter [](https://qpdf.readthedocs.io/en/stable/#option-flatten-annotations "Link to this definition")

This option collapses annotations into the pages’ contents with special handling for form fields. Ordinarily, an annotation is rendered separately and on top of the page. Combining annotations into the page’s contents effectively freezes the placement of the annotations, making them look right after various page transformations. The library functionality backing this option was added for the benefit of programs that want to create *n-up* page layouts and other similar things that don’t work well with annotations. The value of `*parameter*` may be any of the following:

| `all` | include all annotations that are not marked invisible or hidden |
| --- | --- |
| `print` | only include annotations that should appear when the page is printed |
| `screen` | omit annotations that should not appear on the screen |

In a PDF file, interactive form fields have a value and, independently, a set of instructions, called an appearance, to render the filled-in field. If a form is filled in by a program that doesn’t know how to update the appearances, they may become inconsistent with the fields’ values. If qpdf detects this case, its default behavior is not to flatten those annotations because doing so would cause the value of the form field to be lost. This gives you a chance to go back and resave the form with a program that knows how to generate appearances. qpdf itself can generate appearances with some limitations. See the option for details.

\--rotate=\[+|-\]angle\[:page-range\] [](https://qpdf.readthedocs.io/en/stable/#option-rotate "Link to this definition")

Rotate the specified range of pages by the specified angle, which must be a multiple of 90 degrees.

The value of `*angle*` may be `0`, `90`, `180`, or `270`.

For a description of the syntax of `*page-range*`, see. If the page range is omitted, the rotation is applied to all pages.

If `+` is prepended to `*angle*`, the angle is added, so an angle of `+90` indicates a 90-degree clockwise rotation. If `-` is prepended, the angle is subtracted, so `-90` is a 90-degree counterclockwise rotation and is exactly the same as `+270`.

If neither `+` or `-` is prepended, the rotation angle is set exactly. You almost always want `+` or `-` since, without inspecting the actual PDF code, it is impossible to know whether a page that appears to be rotate is rotated “naturally” or has been rotated by specifying rotation. For example, if a page appears to contain a portrait-mode image rotated by 90 degrees so that the top of the image is on the right edge of the page, there is no way to tell by visual inspection whether the literal top of the image is the top of the page or whether the literal top of the image is the right edge and the page is already rotated in the PDF. Specifying a rotation angle of `-90` will produce an image that appears upright in either case. Use of absolute rotation angles should be reserved for cases in which you have specific knowledge about the way the PDF file is constructed.

Examples:

- `qpdf in.pdf out.pdf --rotate=+90:2,4,6 --rotate=+180:7-8`: rotate pages 2, 4, and 6 by 90 degrees clockwise from their original rotation
- `qpdf in.pdf out.pdf --rotate=+180`: rotate all pages by 180 degrees
- `qpdf in.pdf out.pdf --rotate=0`: force each page to be displayed in its natural orientation, which would undo the effect of any rotations previously applied in page metadata.

See also .

\--generate-appearances [](https://qpdf.readthedocs.io/en/stable/#option-generate-appearances "Link to this definition")

If a file contains interactive form fields and indicates that the appearances are out of date with the values of the form, this flag will regenerate appearances, subject to a few limitations. Note that there is usually no reason to do this, but it can be necessary before using the option. Here is a summary of the limitations.

- Radio button and checkbox appearances use the pre-set values in the PDF file. **qpdf** just makes sure that the correct appearance is displayed based on the value of the field. This is fine for PDF files that create their forms properly. Some PDF writers save appearances for fields when they change, which could cause some controls to have inconsistent appearances.
- For text fields and list boxes, any characters that fall outside of US-ASCII or, if detected, “Windows ANSI” or “Mac Roman” encoding, will be replaced by the `?` character.**qpdf** does not know enough about fonts and encodings to correctly represent characters that fall outside of this range.
- For variable text fields where the default appearance stream specifies that the font should be auto-sized, a fixed font size is used rather than calculating the font size.
- Quadding is ignored. Quadding is used to specify whether the contents of a field should be left, center, or right aligned with the field.
- Rich text, multi-line, and other more elaborate formatting directives are ignored.
- There is no support for multi-select fields or signature fields.

Appearances generated by **qpdf** should be good enough for simple forms consisting of ASCII characters where the original file followed the PDF specification and provided template information for text field appearances. If **qpdf** doesn’t do a good enough job with your form, use an external application to save your filled-in form before processing it with **qpdf**. Most PDF viewers that support filling in of forms will generate appearance streams. Some of them will even do it for forms filled in with characters outside the original font’s character range by embedding additional fonts as needed.

\--optimize-images [](https://qpdf.readthedocs.io/en/stable/#option-optimize-images "Link to this definition")

This flag causes qpdf to recompress all images that are not compressed with DCT (JPEG) using DCT compression as long as doing so decreases the size in bytes of the image data and the image does not fall below minimum specified dimensions. Useful information is provided when used in combination with . See also the , , and options. By default, inline images are converted to regular images and optimized as well. Use to prevent inline images from being included. See also .

\--oi-min-width=width [](https://qpdf.readthedocs.io/en/stable/#option-oi-min-width "Link to this definition")

Avoid optimizing images whose width is below the specified amount. If omitted, the default is 128 pixels. Use 0 for no minimum.

\--oi-min-height=height [](https://qpdf.readthedocs.io/en/stable/#option-oi-min-height "Link to this definition")

Avoid optimizing images whose height is below the specified amount. If omitted, the default is 128 pixels. Use 0 for no minimum.

\--oi-min-area=area-in-pixels [](https://qpdf.readthedocs.io/en/stable/#option-oi-min-area "Link to this definition")

Avoid optimizing images whose pixel count ( `*width*`  ×  `*height*` ) is below the specified amount. If omitted, the default is 16,384 pixels. Use 0 for no minimum.

\--keep-inline-images [](https://qpdf.readthedocs.io/en/stable/#option-keep-inline-images "Link to this definition")

Prevent inline images from being included in image optimization done by .

\--remove-info [](https://qpdf.readthedocs.io/en/stable/#option-remove-info "Link to this definition")

Exclude file information (except modification date) from the output file by omitting all entries (except `/ModDate` ) from the `/Info` dictionary in the document trailer. See also .

Exclude metadata from the output file by omitting the `/Metadata` dictionary in the document catalog. See also .

Exclude page labels (explicit page numbers) from the output file by omitting the `/PageLabels` dictionary in the document catalog. See also .

Set page labels (explicit page numbers) for the entire file. This generates a `/PageLabels` dictionary in the document catalog. A PDF file’s pages can be explicitly numbered using page labels. Page labels in a PDF file have an optional type (Arabic numerals, upper/lower-case alphabetic characters, upper/lower-case Roman numerals), an optional prefix, and an optional starting value, which defaults to 1. A qpdf page label spec has the form

`*first-page*:[*type*][/*start*[/*prefix*]]`

where

- `*first-page*` represents a sequential page number using the same format as page ranges (see ): a number, a number preceded by `r` to indicate counting from the end, or `z` indicating the last page
- `*type*` may be one of
	- `D`: Arabic numerals (digits)
	- `A`: Upper-case alphabetic characters
	- `a`: Lower-case alphabetic characters
	- `R`: Upper-case Roman numerals
	- `r`: Lower-case Roman numerals
	- omitted: the page number does not appear, though the prefix, if specified will still appear
- `*start*` must be a number ≥ 1
- `*prefix*` may be any string and is prepended to each page label

The first label spec must have a `*first-page*` value of `1`, indicating the first page of the document. If multiple page label specs are specified, they must be given in increasing order.

A given page label spec causes pages to be numbered according to that scheme starting with `*first-page*` and continuing until the next label spec or the end of the document. If you want to omit numbering starting at a certain page, you can use `*first-page*:` as the spec.

Here are some example page labeling schemes. For these examples, assume a 50-page document.

- `1:a 5:D`
	- The first four pages will be numbered `a` through `d`, then the remaining pages will numbered `1` through `46`.
- `1:r 5:D 12: 14:D/10 r5:D//A- z://"end note"`:
	- The first four pages are numbered `i` through `iv`
	- The 5th page is numbered `1`, and pages are numbered sequentially through the 11th page, which will be numbered `7`
	- The 12th and 13th pages will not have labels
	- The 14th page is numbered `10`. Pages will be numbered sequentially up through the 45th page, which will be numbered `41`
	- Starting with the 46th page (the fifth to last page) and going to the 49th page, pages will be labeled `A-1` through `A-4`
	- The 50th page (the last page) will be labeled `end note`.

The limitations on the range of formats for page labels are as specified in Section 12.4.2 of the PDF spec, ISO 32000.

See also .

## Encryption

This section describes the options used to create encrypted files. For other options related to encryption, see also and . For a more in-depth technical discussion of how PDF encryption works internally, see [PDF Encryption](https://qpdf.readthedocs.io/en/stable/encryption.html#pdf-encryption).

To create an encrypted file, use one of

```
--encrypt \
  [--user-password=user-password] \
  [--owner-password=owner-password] \
  --bits=key-length [options] --
```

OR

```
--encrypt user-password owner-password key-length [options] --
```

The first form, with flags for the passwords and bit length, was introduced in qpdf 11.7.0. Only the option is mandatory. This form allows you to use any text as the password. If passwords are specified, they must be given before the option.

The second form has been in qpdf since the beginning and will continue to be supported. Either or both of user-password and owner-password may be empty strings.

The `key-length` parameter must be either `40`, `128`, or `256`. The user and/or owner password may be omitted. Omitting either password enables the PDF file to be opened without a password. Specifying the same value for the user and owner password and specifying an empty owner password are both considered insecure.

Encryption options are terminated by `--` by itself.

40-bit encryption is insecure, as is 128-bit encryption without AES. Use 256-bit encryption unless you have a specific reason to use an insecure format, such as testing or compatibility with very old viewers. You must use the flag to create encrypted files that use insecure cryptographic algorithms. The flag appears outside of `--encrypt ... --` (before `--encrypt` or after `--` ).

If `*key-length*` is 256, the minimum PDF version is 1.7 with extension level 8, and the AES-based encryption format used is the one described in the PDF 2.0 specification. Using 128-bit encryption forces the PDF version to be at least 1.4, or if AES is used, 1.6. Using 40-bit encryption forces the PDF version to be at least 1.3.

When 256-bit encryption is used, PDF files with empty owner passwords are insecure. To create such files, you must specify the option.

Available options vary by key length. Not all readers respect all restrictions. The default for each permission option is to be fully permissive. These restrictions may or may not be enforced by any particular reader. **qpdf** allows very granular setting of restrictions. Some readers may not recognize the combination of options you specify. If you specify certain combinations of restrictions and find a reader that doesn’t seem to honor them as you expect, it is most likely not a bug in **qpdf**. qpdf itself does not obey encryption restrictions already imposed on the file. Doing so would be meaningless since qpdf can be used to remove encryption from the file entirely.

Here is a summary of encryption options. Details are provided in the help for each option.

| `--annotate=[y\|n]` | restrict comments, filling forms, and signing |
| --- | --- | --- |
| `--extract=[y\|n]` | restrict text/graphic extraction |
| `--modify=[y\|n]` | restrict document modification |
| `--print=[y\|n]` | restrict printing |

| `--accessibility=[y\|n]` | restrict accessibility (usually ignored) |
| --- | --- | --- |
| `--annotate=[y\|n]` | restrict commenting/filling form fields |
| `--assemble=[y\|n]` | restrict document assembly |
| `--extract=[y\|n]` | restrict text/graphic extraction |
| `--form=[y\|n]` | restrict filling form fields |
| `--modify-other=[y\|n]` | restrict other modifications |
| `--modify=modify-opt` | control modify access by level |
| `--print=print-opt` | control printing access |
| `--cleartext-metadata` | prevent encryption of metadata |

| `--use-aes=[y\|n]` | indicates whether to use AES encryption |
| --- | --- | --- |
| `--force-V4` | forces use of V=4 encryption handler |

| `--force-R5` | forces use of deprecated `R=5` encryption algorithm |
| --- | --- |
| `--allow-insecure` | allow user password with empty owner password |

| `none` | disallow printing |
| --- | --- |
| `low` | allow only low-resolution printing |
| `full` | allow full printing |

| `none` | allow no modifications |
| --- | --- |
| `assembly` | allow document assembly only |
| `form` | `assembly` permissions plus filling in form fields and signing |
| `annotate` | `form` permissions plus commenting and modifying forms |
| `all` | allow full document modification |

### Related Options

\--user-password=user-password [](https://qpdf.readthedocs.io/en/stable/#option-user-password "Link to this definition")

Set the user password of the encrypted file. Conforming readers apply security restrictions to files opened with the user password.

\--owner-password=owner-password [](https://qpdf.readthedocs.io/en/stable/#option-owner-password "Link to this definition")

Set the owner password of the encrypted file. Conforming readers allow security restrictions to be changed or overridden when files are opened with the owner password.

\--bits={48|128|256} [](https://qpdf.readthedocs.io/en/stable/#option-bits "Link to this definition")

Set the key length for encrypted files. You should always use `--bits=256` unless you have a strong reason to create a file with weaker encryption.

\--accessibility=\[y|n\] [](https://qpdf.readthedocs.io/en/stable/#option-accessibility "Link to this definition")

Enable/disable extraction of text for accessibility to visually impaired users. The default is to be fully permissive. The qpdf library disregards this field when AES is used with 128-bit encryption or when 256-bit encryption is used. You should never disable accessibility unless you are explicitly doing so for creating test files. The PDF spec says that conforming readers should disregard this permission and always allow accessibility.

This option is not available with 40-bit encryption.

\--annotate=\[y|n\] [](https://qpdf.readthedocs.io/en/stable/#option-annotate "Link to this definition")

Enable/disable modifying annotations including making comments and filling in form fields. The default is to be fully permissive. For 128-bit and 256-bit encryption, this also enables editing, creating, and deleting form fields unless `--modify-other=n` or `--modify=none` is also specified.

\--assemble=\[y|n\] [](https://qpdf.readthedocs.io/en/stable/#option-assemble "Link to this definition")

Enable/disable document assembly (rotation and reordering of pages). The default is to be fully permissive.

This option is not available with 40-bit encryption.

\--extract=\[y|n\] [](https://qpdf.readthedocs.io/en/stable/#option-extract "Link to this definition")

Enable/disable text/graphic extraction for purposes other than accessibility. The default is to be fully permissive.

\--form=\[y|n\] [](https://qpdf.readthedocs.io/en/stable/#option-form "Link to this definition")

Enable/disable whether filling form fields is allowed even if modification of annotations is disabled. The default is to be fully permissive.

This option is not available with 40-bit encryption.

\--modify-other=\[y|n\] [](https://qpdf.readthedocs.io/en/stable/#option-modify-other "Link to this definition")

Enable/disable modifications not controlled by, , or. `--modify-other=n` is implied by any of the other options except for `--modify=all`. The default is to be fully permissive.

This option is not available with 40-bit encryption.

\--modify=modify-opt [](https://qpdf.readthedocs.io/en/stable/#option-modify "Link to this definition")

For 40-bit files, `*modify-opt*` may only be `y` or `n` and controls all aspects of document modification. The default is to be fully permissive.

For 128-bit and 256-bit encryption, `*modify-opt*` values allow enabling and disabling levels of restriction in a manner similar to how some PDF creation tools do it:

| `none` | allow no modifications |
| --- | --- |
| `assembly` | allow document assembly only |
| `form` | `assembly` permissions plus filling in form fields and signing |
| `annotate` | `form` permissions plus commenting and modifying forms |
| `all` | allow full document modification (the default) |

Modify options correspond to the more granular options as follows:

| `none` | `--modify-other=n --annotate=n --form=n --assemble=n` |
| --- | --- |
| `assembly` | `--modify-other=n --annotate=n --form=n` |
| `form` | `--modify-other=n --annotate=n` |
| `annotate` | `--modify-other=n` |
| `all` | no other modify options (the default) |

You can combine this option with the options listed above. If you do, later options override earlier options.

\--print=print-opt [](https://qpdf.readthedocs.io/en/stable/#option-print "Link to this definition")

Control what kind of printing is allowed. The default is to be fully permissive. For 40-bit encryption, `*print-opt*` may only be `y` or `n` and enables or disables all printing. For 128-bit and 256-bit encryption, `*print-opt*` may have the following values:

| `none` | disallow printing |
| --- | --- |
| `low` | allow low-resolution printing only |
| `full` | allow full printing (the default) |

If specified, any metadata stream in the document will be left unencrypted even if the rest of the document is encrypted. This also forces the PDF version to be at least 1.5.

This option is not available with 40-bit encryption.

\--use-aes=\[y|n\] [](https://qpdf.readthedocs.io/en/stable/#option-use-aes "Link to this definition")

Enables/disables use of the more secure AES encryption with 128-bit encryption. Specifying `--use-aes=y` forces the PDF version to be at least 1.6. This option is only available with 128-bit encryption. The default is `n` for compatibility reasons. Use 256-bit encryption instead.

\--allow-insecure [](https://qpdf.readthedocs.io/en/stable/#option-allow-insecure "Link to this definition")

Allow creation of PDF files with 256-bit keys where the user password is non-empty and the owner password is empty. Files created in this way are insecure since they can be opened without a password, and restrictions will not be enforced. Users would ordinarily never want to create such files. If you are using qpdf to intentionally create strange files for testing (a valid use of qpdf!), this option allows you to create such insecure files. This option is only available with 256-bit encryption.

See [User and Owner Passwords](https://qpdf.readthedocs.io/en/stable/encryption.html#pdf-passwords) for a more technical discussion of this issue.

\--force-V4 [](https://qpdf.readthedocs.io/en/stable/#option-force-V4 "Link to this definition")

Use of this option forces the `V` and `R` parameters in the document’s encryption dictionary to be set to the value `4`. As qpdf will automatically do this when required, there is no reason to ever use this option. It exists primarily for use in testing qpdf itself. This option also forces the PDF version to be at least 1.5.

\--force-R5 [](https://qpdf.readthedocs.io/en/stable/#option-force-R5 "Link to this definition")

Use an undocumented, unsupported, deprecated encryption algorithm that existed only in Acrobat version IX. This option should not be used except for compatibility testing. If specified, qpdf sets the minimum version to 1.7 at extension level 3.

## Page Selection

**qpdf** allows you to use the option to split and merge PDF files by selecting pages from one or more input files.

```
qpdf primary-input.pdf \
  --pages \
  --file=input.pdf \
  [--range=page-range] \
  [--password=password] \
  [...] \
  -- output.pdf
```

OR

```
qpdf primary-input.pdf \
  --pages \
  input.pdf [--password=password] [page-range] \
  [...] -- output.pdf
```

Notes:

- The first form, with and , was introduced in qpdf 11.9.0. In this form, the and options apply to the most recently specified option.
- The password option is needed only for password-protected files. If you specify the same file more than once, you only need to supply the password the first time.
- The page range may be omitted. If omitted, all pages are included.
- Document-level information, such as outlines, tags, etc., is taken from the primary input file (in the above example, `in.pdf` ) and is preserved in `out.pdf`. You can use in place of an input file to start from an empty file and just copy pages equally from all files.
- You can use `.` as a shorthand for the primary input file, if not empty.

See for help on specifying a page range.

Use `--collate=*n*` to cause pages to be collated in groups of `*n*` pages (default 1) instead of concatenating the input. Use `--collate=*i*,*j*,*k*,...` to take `*i*` from the first, then `*j*` from the second, then `*k*` from the third, then `*i*` from the first, etc.

Note that the appears outside of `--pages ... --` (before `--pages` or after `--` ). Pages are pulled from each document in turn. When a document is out of pages, it is skipped. See examples below.

### Examples

- Start with `in.pdf` and append all pages from `a.pdf` and the even pages from `b.pdf`, and write the output to `out.pdf`. Document-level information from `in.pdf` is retained. Note the use of `.` to refer to `in.pdf`.
	```
	qpdf in.pdf --pages . a.pdf b.pdf 1-z:even -- out.pdf
	```
- Take all the pages from `a.pdf`, all the pages from `b.pdf` in reverse, and only pages 3 and 6 from `c.pdf` and write the result to `out.pdf`. Document-level metadata is discarded from all input files. The password `x` is used to open `b.pdf`.
	```
	qpdf --empty --pages --file=a.pdf \
	  --file=b.pdf --password=x --range=z-1 \
	  --file=c.pdf --range=3,6 -- out.pdf
	```
- Scan a document with double-sided printing by scanning the fronts into `odd.pdf` and the backs into `even.pdf`. Collate the results into `all.pdf`. This takes the first page of `odd.pdf`, the first page of `even.pdf`, the second page of `odd.pdf`, the second page of `even.pdf`, etc.
	```
	qpdf --collate odd.pdf --pages . even.pdf -- all.pdf
	  OR
	qpdf --collate --empty --pages odd.pdf even.pdf -- all.pdf
	  OR
	qpdf --collate --empty --pages --file=odd.pdf --file=even.pdf -- all.pdf
	```
- When collating, any number of files and page ranges can be specified. If any file has fewer pages, that file is just skipped when its pages have all been included. For example, if you ran
	```
	qpdf --collate --empty --pages a.pdf 1-5 b.pdf 6-4 c.pdf r1 -- out.pdf
	```
	you would get the following pages in this order:
	- a.pdf page 1
	- b.pdf page 6
	- c.pdf last page
	- a.pdf page 2
	- b.pdf page 5
	- a.pdf page 3
	- b.pdf page 4
	- a.pdf page 4
	- a.pdf page 5
- You can specify a numeric parameter to . With `--collate=*n*`, pull groups of `*n*` pages from each file, as always, stopping when there are no more pages. For example, if you ran
	```
	qpdf --collate=2 --empty --pages a.pdf 1-5 b.pdf 6-4 c.pdf r1 -- out.pdf
	```
	you would get the following pages in this order:
	- a.pdf page 1
	- a.pdf page 2
	- b.pdf page 6
	- b.pdf page 5
	- c.pdf last page
	- a.pdf page 3
	- a.pdf page 4
	- b.pdf page 4
	- a.pdf page 5
- You can specify a multiple numeric parameters to . With `--collate=*i,j,k*`, pull groups of `*i*` pages from the first file, then `*j*` from the second, then `*k*` from the third, repeating. The number of parameters must equal the number of groups. For example, if you ran
	```
	qpdf --collate=2,1,3 --empty --pages a.pdf 1-5 b.pdf 6-4 c.pdf r1-r4 -- out.pdf
	```
	you would get the following pages in this order:
	- a.pdf pages 1 and 2
	- b.pdf page 6
	- c.pdf last three pages in reverse order
	- a.pdf pages 3 and 4
	- b.pdf page 5
	- c.pdf fourth to last page
	- a.pdf page 5
	- b.pdf page 4
- Take pages 1 through 5 from `file1.pdf` and pages 11 through 15 in reverse from `file2.pdf`, taking document-level metadata from `file2.pdf`.
	```
	qpdf file2.pdf --pages file1.pdf 1-5 . 15-11 -- outfile.pdf
	```
- Here’s a more contrived example. If, for some reason, you wanted to take the first page of an encrypted file called `encrypted.pdf` with password `pass` and repeat it twice in an output file without any shared data between the two copies of page 1, and if you wanted to drop document-level metadata but preserve encryption, you could run
	```
	qpdf --empty --copy-encryption=encrypted.pdf \
	     --encryption-file-password=pass \
	     --pages encrypted.pdf --password=pass 1 \
	           ./encrypted.pdf --password=pass 1 -- \
	     outfile.pdf
	```
	Note that we had to specify the password all three times because giving a password as doesn’t count for page selection, and as far as qpdf is concerned,`encrypted.pdf` and `./encrypted.pdf` are separate files. (This is by design. See for a discussion.) These are all corner cases that most users should hopefully never have to be bothered with.

### Limitations

With the exception of page labels (page numbers), **qpdf** doesn’t yet have full support for handling document-level data as it relates to pages. Certain document-level features such as form fields, outlines (bookmarks), and article tags among others, are copied in their entirety from the primary input file. Starting with qpdf version 8.3, page labels are preserved from all files unless is specified.

It is expected that a future version of **qpdf** will have more complete and configurable behavior regarding document-level metadata. In the meantime, semantics of splitting and merging vary across features. For example, the document’s outlines (bookmarks) point to actual page objects, so if you select some pages and not others, bookmarks that point to pages that are in the output file will work, and remaining bookmarks will not work. If you don’t want to preserve the primary file’s metadata, use as the primary input file.

Visit [qpdf issues labeled with “pages”](https://github.com/qpdf/qpdf/issues?q=is%3Aopen+is%3Aissue+label%3Apages) or look at the `TODO` file in the qpdf source distribution for some of the ideas.

Prior to **qpdf** version 8.4, it was not possible to specify the same page from the same file directly more than once, and a workaround of specifying the same file in more than one way was required. Version 8.4 removes this limitation, but when the same page is copied more than once, all its data is shared between the pages. Sometimes this is fine, but sometimes it may not work correctly, particularly if there are form fields or you intend to perform other modifications on one of the pages. A future version of qpdf should address this more completely. You can work around this by specifying the same file in two different ways. For example **qpdf in.pdf --pages. 1./in.pdf 1 -- out.pdf** would create a file with two copies of the first page of the input, and the two copies would not share any objects in common. This includes fonts, images, and anything else the page references.

## Embedded Files/Attachments

It is possible to list, add, or delete embedded files (also known as attachments) and to copy attachments from other files. See also and .

### Related Options

\--add-attachment file \[options\] \-- [](https://qpdf.readthedocs.io/en/stable/#option-add-attachment "Link to this definition")

This flag starts add attachment options, which are used to add attachments to a file.

The `--add-attachment` flag and its options may be repeated to add multiple attachments. Please see for additional details.

\--copy-attachments-from file \[options\] \-- [](https://qpdf.readthedocs.io/en/stable/#option-copy-attachments-from "Link to this definition")

This flag starts copy attachment options, which are used to copy attachments from other files.

The `--copy-attachments-from` flag and its options may be repeated to copy attachments from multiple files. Please see for additional details.

\--remove-attachment=key [](https://qpdf.readthedocs.io/en/stable/#option-remove-attachment "Link to this definition")

Remove the specified attachment. This doesn’t only remove the attachment from the embedded files table but also clears out the file specification to ensure that the attachment is actually not present in the output file. That means that any potential internal links to the attachment will be broken. Run with to see status of the removal. Use to find the attachment key. This option may be repeated to remove multiple attachments.

### PDF Date Format

When a date is required, the date should conform to the PDF date format specification, which is `D:*yyyymmddhhmmssz*` where `*z*` is either literally upper case `Z` for UTC or a timezone offset in the form `*-hh'mm'*` or `*+hh'mm'*`. Negative timezone offsets indicate time before UTC. Positive offsets indicate how far after. For example, US Eastern Standard Time (America/New\_York) is `-05'00'`, and Indian Standard Time (Asia/Calcutta) is `+05'30'`.

| `D:20210207161528-05'00'` | February 7, 2021 at 4:15:28 p.m. |
| --- | --- |
| `D:20210207211528Z` | February 7, 2021 at 21:15:28 UTC |

### Options for Adding Attachments

These options are valid between and `--`.

\--key=key [](https://qpdf.readthedocs.io/en/stable/#option-key "Link to this definition")

Specify the key to use for the attachment in the embedded files table. It defaults to the last element of the attached file’s filename. For example, if you say `--add-attachment /home/user/image.png`, the default key will be just `image.png`.

\--filename=name [](https://qpdf.readthedocs.io/en/stable/#option-filename "Link to this definition")

Specify the filename to be used for the attachment. This is what is usually displayed to the user and is the name most graphical PDF viewers will use when saving a file. It defaults to the last element of the attached file’s filename. For example, if you say `--add-attachment /home/user/image.png`, the default key will be just `image.png`.

\--creationdate=date [](https://qpdf.readthedocs.io/en/stable/#option-creationdate "Link to this definition")

Specify the attachment’s creation date in PDF format; defaults to the current time. See for information about the date format.

\--moddate=date [](https://qpdf.readthedocs.io/en/stable/#option-moddate "Link to this definition")

Specify the attachment’s modification date in PDF format; defaults to the current time. See for information about the date format.

\--mimetype=type/subtype [](https://qpdf.readthedocs.io/en/stable/#option-mimetype "Link to this definition")

Specify the mime type for the attachment, such as `text/plain`,`application/pdf`, `image/png`, etc. The qpdf library does not automatically determine the mime type. In a UNIX-like environment, the **file** command can often provide this information. In MacOS, you can use `file -I *filename*`. In Linux, it’s `file -i *filename*`.

Implementation note: the mime type appears in a field called `/Subtype` in the PDF file, but that field actually includes the full type and subtype of the mime type. This is because `/Type` already means something else in PDF.

\--description="text" [](https://qpdf.readthedocs.io/en/stable/#option-description "Link to this definition")

Supply descriptive text for the attachment, displayed by some PDF viewers.

\--replace [](https://qpdf.readthedocs.io/en/stable/#option-replace "Link to this definition")

Indicate that any existing attachment with the same key should be replaced by the new attachment. Otherwise, **qpdf** gives an error if an attachment with that key is already present.

### Options for Copying Attachments

Options in this section are valid between and `--`.

\--prefix=prefix [](https://qpdf.readthedocs.io/en/stable/#option-prefix "Link to this definition")

Only required if the file from which attachments are being copied has attachments with keys that conflict with attachments already in the file. In this case, the specified prefix will be prepended to each key. This affects only the key in the embedded files table, not the file name. The PDF specification doesn’t preclude multiple attachments having the same file name.

## PDF Inspection

These options provide tools for inspecting PDF files. When any of the options in this section are specified, no output file may be given.

### Related Options

\--is-encrypted [](https://qpdf.readthedocs.io/en/stable/#option-is-encrypted "Link to this definition")

Silently exit with a code indicating the file’s encryption status:

| `0` | the file is encrypted |
| --- | --- |
| `1` | not used |
| `2` | the file is not encrypted |

This option can be used for password-protected files even if you don’t know the password.

This option is useful for shell scripts. Other options are ignored if this is given. This option is mutually exclusive with. Both this option and exit with status `2` for non-encrypted files.

\--requires-password [](https://qpdf.readthedocs.io/en/stable/#option-requires-password "Link to this definition")

Silently exit with a code indicating the file’s password status:

| `0` | a password, other than as supplied, is required |
| --- | --- |
| `1` | not used |
| `2` | the file is not encrypted |
| `3` | the file is encrypted, and correct password (if any) has been supplied |

Use with the option to specify the password to test.

The choice of exit status `0` to mean that a password is required is to enable code like

```
if qpdf --requires-password file.pdf; then
    # prompt for password
fi
```

If a password is supplied with , that password is used to open the file just as with any normal invocation of **qpdf**. That means that using this option with can be used to check the correctness of the password. In that case, an exit status of `3` means the file works with the supplied password. This option is mutually exclusive with . Both this option and exit with status `2` for non-encrypted files.

\--check [](https://qpdf.readthedocs.io/en/stable/#option-check "Link to this definition")

Check the file’s structure as well as encryption, linearization, and encoding of stream data, and write information about the file to standard output. An exit status of `0` indicates syntactic correctness of the PDF file. Note that `--check` writes nothing to standard error when everything is valid, so if you are using this to programmatically validate files in bulk, it is safe to run without output redirected to `/dev/null` and just check for a `0` exit code.

A file for which `--check` reports no errors may still have errors in stream data content or may contain constructs that don’t conform to the PDF specification, but it should be syntactically valid. If `--check` reports any errors, qpdf will exit with a status of `2`. There are some recoverable conditions that `--check` detects. These are issued as warnings instead of errors. If qpdf finds no errors but finds warnings, it will exit with a status of `3`. When `--check` is combined with other options, checks are always performed before any other options are processed. For erroneous files, `--check` will cause qpdf to attempt to recover, after which other options are effectively operating on the recovered file. Combining `--check` with other options in this way can be useful for manually recovering severely damaged files.

See also .

\--show-encryption [](https://qpdf.readthedocs.io/en/stable/#option-show-encryption "Link to this definition")

This option shows document encryption parameters. It also shows the document’s user password if the owner password is given and the file was encrypted using older encryption formats that allow user password recovery. (See [PDF Encryption](https://qpdf.readthedocs.io/en/stable/encryption.html#pdf-encryption) for a technical discussion of this feature.) The output of `--show-encryption` is included in the output of .

\--show-encryption-key [](https://qpdf.readthedocs.io/en/stable/#option-show-encryption-key "Link to this definition")

When encryption information is being displayed, as when or is given, display the computed or retrieved encryption key as a hexadecimal string. This value is not ordinarily useful to users, but it can be used as the parameter to if the is specified. Note that, when PDF files are encrypted, passwords and other metadata are used only to compute an encryption key, and the encryption key is what is actually used for encryption. This enables retrieval of that key. See [PDF Encryption](https://qpdf.readthedocs.io/en/stable/encryption.html#pdf-encryption) for a technical discussion.

\--check-linearization [](https://qpdf.readthedocs.io/en/stable/#option-check-linearization "Link to this definition")

Check to see whether a file is linearized and, if so, whether the linearization hint tables are correct. qpdf does not check all aspects of linearization. A linearized PDF file with linearization errors that is otherwise correct is almost always readable by a PDF viewer. As such, “errors” in PDF linearization are treated by **qpdf** as warnings.

\--show-linearization [](https://qpdf.readthedocs.io/en/stable/#option-show-linearization "Link to this definition")

Check and display all data in the linearization hint tables.

\--show-xref [](https://qpdf.readthedocs.io/en/stable/#option-show-xref "Link to this definition")

Show the contents of the cross-reference table or stream in a human-readable form. The cross-reference data gives the offset of regular objects and the object stream ID and 0-based index within the object stream for compressed objects. This is especially useful for files with cross-reference streams, which are stored in a binary format. If the file is invalid and cross reference table reconstruction is performed, this option will show the information in the reconstructed table.

\--show-object={trailer|obj\[,gen\]} [](https://qpdf.readthedocs.io/en/stable/#option-show-object "Link to this definition")

Show the contents of the given object. This is especially useful for inspecting objects that are inside of object streams (also known as “compressed objects”).

\--raw-stream-data [](https://qpdf.readthedocs.io/en/stable/#option-raw-stream-data "Link to this definition")

When used with , if the object is a stream, write the raw (compressed) binary stream data to standard output instead of the object’s contents. Avoid combining this with other inspection options to avoid commingling the stream data with other output. See also .

\--filtered-stream-data [](https://qpdf.readthedocs.io/en/stable/#option-filtered-stream-data "Link to this definition")

When used with , if the object is a stream, write the filtered (uncompressed, potentially binary) stream data to standard output instead of the object’s contents. If the stream is filtered using filters that qpdf does not support, an error will be issued. This option acts as if `--decode-level=all` was specified (see ), so it will uncompress images compressed with supported lossy compression schemes. Avoid combining this with other inspection options to avoid commingling the stream data with other output.

This option may be combined with . If you do this, qpdf will attempt to run content normalization even if the stream is not a content stream, which will probably produce unusable results.

See also .

\--show-npages [](https://qpdf.readthedocs.io/en/stable/#option-show-npages "Link to this definition")

Print the number of pages in the input file on a line by itself. Since the number of pages appears by itself on a line, this option can be useful for scripting if you need to know the number of pages in a file.

\--show-pages [](https://qpdf.readthedocs.io/en/stable/#option-show-pages "Link to this definition")

Show the object and generation number for each page dictionary object and for each content stream associated with the page. Having this information makes it more convenient to inspect objects from a particular page. See also .

\--with-images [](https://qpdf.readthedocs.io/en/stable/#option-with-images "Link to this definition")

When used with , also shows the object and generation numbers for the image objects on each page.

\--list-attachments [](https://qpdf.readthedocs.io/en/stable/#option-list-attachments "Link to this definition")

Show the *key* and stream number for each embedded file. With, additional information, including preferred file name, description, dates, and more are also displayed. The key is usually but not always equal to the file name and is needed by some of the other options. See also . Note that this option displays dates in PDF timestamp syntax. When attachment information is included in json output in the `"attachments"` key (see ), dates are shown (just within that object) in ISO-8601 format.

\--show-attachment=key [](https://qpdf.readthedocs.io/en/stable/#option-show-attachment "Link to this definition")

Write the contents of the specified attachment to standard output as binary data. The key should match one of the keys shown by. If this option is given more than once, only the last attachment will be shown. See also.

## JSON Options

It is possible to view information about PDF files in a JSON format. See [qpdf JSON](https://qpdf.readthedocs.io/en/stable/json.html#json) for details about the qpdf JSON format.

### Related Options

\--json\[=version\] [](https://qpdf.readthedocs.io/en/stable/#option-json "Link to this definition")

Generate a JSON representation of the file. This is described in depth in [qpdf JSON](https://qpdf.readthedocs.io/en/stable/json.html#json). The version parameter can be used to specify which version of the qpdf JSON format should be output. The version number be a number or `latest`. The default is `latest`. As of qpdf 11, the latest version is `2`. If you have code that reads qpdf JSON output, you can tell what version of the JSON output you have from the `"version"` key in the output. Use the option to get a description of the JSON object.

Starting with qpdf 11, when this option is specified, an output file is optional (for backward compatibility) and defaults to standard output. You may specify an output file to write the JSON to a file rather than standard output. (Example: `qpdf --json in.pdf out.json` )

Stream data is only included if is specified or if a value other than `none` is passed to.

\--json-help\[=version\] [](https://qpdf.readthedocs.io/en/stable/#option-json-help "Link to this definition")

Describe the format of the corresponding version of JSON output by writing to standard output a JSON object with the same structure as the JSON generated by qpdf. In the output written by `--json-help`, each key’s value is a description of the key. The specific contract guaranteed by qpdf in its JSON representation is explained in more detail in the [qpdf JSON](https://qpdf.readthedocs.io/en/stable/json.html#json). The default version of help is version `2`, as with the flag.

\--json-key=key [](https://qpdf.readthedocs.io/en/stable/#option-json-key "Link to this definition")

This option is repeatable. If given, only the specified top-level keys will be included in the JSON output. Otherwise, all keys will be included. If not given, all keys will be included, unless was specified, in which case, only the `"qpdf"` key will be included by default. If was not given, the `version` and `parameters` keys will always appear in the output.

\--json-object={trailer|obj\[,gen\]} [](https://qpdf.readthedocs.io/en/stable/#option-json-object "Link to this definition")

This option is repeatable. If given, only specified objects will be shown in the objects dictionary in the JSON output. Otherwise, all objects will be shown. See [qpdf JSON](https://qpdf.readthedocs.io/en/stable/json.html#json) for details about the qpdf JSON format.

\--json-stream-data={none|inline|file} [](https://qpdf.readthedocs.io/en/stable/#option-json-stream-data "Link to this definition")

When used with , this option controls whether streams in JSON output should be omitted, written inline (base64-encoded) or written to a file. If `file` is chosen, the file will be the name of the output file appended with `-*nnn*` where `*nnn*` is the object number. The stream data file prefix can be overridden with. The default value is `none`, except when is specified, in which case the default is `inline`.

\--json-stream-prefix=file-prefix [](https://qpdf.readthedocs.io/en/stable/#option-json-stream-prefix "Link to this definition")

When used with `--json-stream-data=file`,`--json-stream-data=file-prefix` sets the prefix for stream data files, overriding the default, which is to use the output file name. Whatever is given here will be appended with `-*nnn*` to create the name of the file that will contain the data for the stream stream in object `*nnn*`.

\--json-output\[=version\] [](https://qpdf.readthedocs.io/en/stable/#option-json-output "Link to this definition")

Implies at the specified version. This option changes several default values, all of which can be overridden by specifying the stated option:

- The default value for changes from `none` to `inline`.
- The default value for changes from `generalized` to `none`.
- By default, only the `"qpdf"` key is included in the JSON output, but you can add additional keys with.
- The `"version"` and `"parameters"` keys will be excluded from the JSON output.

If you want to look at the contents of streams easily as you would in QDF mode (see [QDF Mode](https://qpdf.readthedocs.io/en/stable/qdf.html#qdf) ), you can use `--decode-level=generalized` and `--json-stream-data=file` for a convenient way to do that.

\--json-input [](https://qpdf.readthedocs.io/en/stable/#option-json-input "Link to this definition")

Treat the input file as a JSON file in qpdf JSON format. The input file must be complete and include all stream data. The JSON version must be at least 2. All top-level keys are ignored except for `"qpdf"`. For information about converting between PDF and JSON, please see [qpdf JSON](https://qpdf.readthedocs.io/en/stable/json.html#json).

\--update-from-json=qpdf-json-file [](https://qpdf.readthedocs.io/en/stable/#option-update-from-json "Link to this definition")

This option updates a PDF file from the specified qpdf JSON file. For a information about how to use this option, please see [qpdf JSON](https://qpdf.readthedocs.io/en/stable/json.html#json).

## Options for Testing or Debugging

The options below are useful when writing automated test code that includes files created by qpdf or when testing qpdf itself. When changes are made to qpdf, care is taken to avoid gratuitously changing the output of PDF files. This is to make it easier to do direct comparisons in test suites with files created by qpdf. However, there are no guarantees that the PDF output won’t change such as in the event of a bug fix or feature enhancement to some aspect of the output that qpdf creates.

### Idempotency

Note about idempotency of byte-for-byte content: there is no expectation that qpdf is idempotent in the general case. In other words, there is no expectation that, when qpdf is run on its own output, it will create *byte-for-byte* identical output, even though it will create semantically identical files. There are a variety of reasons for this including document ID generation, which includes a random element, as well as the interaction of stream length encoding with dictionary key sorting.

It is possible to get idempotent behavior by using the (or, for testing only,) option with qpdf and running it *three* times so that you are processing the output of qpdf on its own previous output. For example, in this sequence of commands:

```
qpdf any-file.pdf 1.pdf
qpdf --deterministic-id 1.pdf 2.pdf
qpdf --deterministic-id 2.pdf 3.pdf
```

the files `2.pdf` and `3.pdf` should be *byte-for-byte* identical. The qpdf test suite relies on this behavior. See also, which should also be used only for testing.

### Related Options

\--static-id [](https://qpdf.readthedocs.io/en/stable/#option-static-id "Link to this definition")

Use a fixed value for the document ID ( `/ID` in the trailer).**This is intended for testing only. Never use it for production files.** If you are trying to get the same ID each time for a given file and you are not generating encrypted files, consider using the option.

\--static-aes-iv [](https://qpdf.readthedocs.io/en/stable/#option-static-aes-iv "Link to this definition")

Use a static initialization vector for AES-CBC. This is intended for testing only so that output files can be reproducible. Never use it for production files. **This option in particular is not secure since it significantly weakens the encryption.** When combined with and using the three-step process described in , it is possible to create byte-for-byte idempotent output with PDF files that use 256-bit encryption to assist with creating reproducible test suites.

\--linearize-pass1=file [](https://qpdf.readthedocs.io/en/stable/#option-linearize-pass1 "Link to this definition")

Write the first pass of linearization to the named file. *The resulting file is not a valid PDF file.* This option is useful only for debugging `QPDFWriter` ’s linearization code. When qpdf linearizes files, it writes the file in two passes, using the first pass to calculate sizes and offsets that are required for hint tables and the linearization dictionary. Ordinarily, the first pass is discarded. This option enables it to be captured, allowing inspection of the file before values calculated in pass 1 are inserted into the file for pass 2.

\--test-json-schema [](https://qpdf.readthedocs.io/en/stable/#option-test-json-schema "Link to this definition")

This is used by qpdf’s test suite to check consistency between the output of `qpdf --json` and the output of `qpdf --json-help`. This option causes an extra copy of the generated JSON to appear in memory and is therefore unsuitable for use with large files. This is why it’s also not on by default.

\--report-memory-usage [](https://qpdf.readthedocs.io/en/stable/#option-report-memory-usage "Link to this definition")

This is used by qpdf’s performance test suite to report the maximum amount of memory used in supported environments.

## Unicode Passwords

At the library API level, all methods that perform encryption and decryption interpret passwords as strings of bytes. It is up to the caller to ensure that they are appropriately encoded. Starting with qpdf version 8.4.0, qpdf will attempt to make this easier for you when interacting with qpdf via its command line interface. The PDF specification requires passwords used to encrypt files with 40-bit or 128-bit encryption to be encoded with PDF Doc encoding. This encoding is a single-byte encoding that supports ISO-Latin-1 and a handful of other commonly used characters. It has a large overlap with Windows ANSI but is not exactly the same. There is generally no way to provide PDF Doc encoded strings on the command line. As such, qpdf versions prior to 8.4.0 would often create PDF files that couldn’t be opened with other software when given a password with non-ASCII characters to encrypt a file with 40-bit or 128-bit encryption. Starting with qpdf 8.4.0, qpdf recognizes the encoding of the parameter and transcodes it as needed. The rest of this section provides the details about exactly how qpdf behaves. Most users will not need to know this information, but it might be useful if you have been working around qpdf’s old behavior or if you are using qpdf to generate encrypted files for testing other PDF software.

A note about Windows: when qpdf builds, it attempts to determine what it has to do to use `wmain` instead of `main` on Windows. The `wmain` function is an alternative entry point that receives all arguments as UTF-16-encoded strings. When qpdf starts up this way, it converts all the strings to UTF-8 encoding and then invokes the regular main. This means that, as far as qpdf is concerned, it receives its command-line arguments with UTF-8 encoding, just as it would in any modern Linux or UNIX environment.

If a file is being encrypted with 40-bit or 128-bit encryption and the supplied password is not a valid UTF-8 string, qpdf will fall back to the behavior of interpreting the password as a string of bytes. If you have old scripts that encrypt files by passing the output of **iconv** to qpdf, you no longer need to do that, but if you do, qpdf should still work. The only exception would be for the extremely unlikely case of a password that is encoded with a single-byte encoding but also happens to be valid UTF-8. Such a password would contain strings of even numbers of characters that alternate between accented letters and symbols. In the extremely unlikely event that you are intentionally using such passwords and qpdf is thwarting you by interpreting them as UTF-8, you can use `--password-mode=bytes` to suppress qpdf’s automatic behavior.

The option, as described earlier in this chapter, can be used to change qpdf’s interpretation of supplied passwords. There are very few reasons to use this option. One would be the unlikely case described in the previous paragraph in which the supplied password happens to be valid UTF-8 but isn’t supposed to be UTF-8. Your best bet would be just to provide the password as a valid UTF-8 string, but you could also use `--password-mode=bytes`. Another reason to use `--password-mode=bytes` would be to intentionally generate PDF files encrypted with passwords that are not properly encoded. The qpdf test suite does this to generate invalid files for the purpose of testing its password recovery capability. If you were trying to create intentionally incorrect files for a similar purposes, the `bytes` password mode can enable you to do this.

When qpdf attempts to decrypt a file with a password that contains non-ASCII characters, it will generate a list of alternative passwords by attempting to interpret the password as each of a handful of different coding systems and then transcode them to the required format. This helps to compensate for the supplied password being given in the wrong coding system, such as would happen if you used the **iconv** workaround that was previously needed. It also generates passwords by doing the reverse operation: translating from correct in incorrect encoding of the password. This would enable qpdf to decrypt files using passwords that were improperly encoded by whatever software encrypted the files, including older versions of qpdf invoked without properly encoded passwords. The combination of these two recovery methods should make qpdf transparently open most encrypted files with the password supplied correctly but in the wrong coding system. There are no real downsides to this behavior, but if you don’t want qpdf to do this, you can use the option. One reason to do that is to ensure that you know the exact password that was used to encrypt the file.

With these changes, qpdf now generates compliant passwords in most cases. There are still some exceptions. In particular, the PDF specification directs compliant writers to normalize Unicode passwords and to perform certain transformations on passwords with bidirectional text. Implementing this functionality requires using a real Unicode library like ICU. If a client application that uses qpdf wants to do this, the qpdf library will accept the resulting passwords, but qpdf will not perform these transformations itself. It is possible that this will be addressed in a future version of qpdf. The `QPDFWriter` methods that enable encryption on the output file accept passwords as strings of bytes.

Please note that the option is unrelated to all this. That flag bypasses the normal process of going from password to encryption key entirely, allowing the raw encryption key to be specified directly. That behavior is useful for forensic purposes or for brute-force recovery of files with unknown passwords and has nothing to do with the document’s actual passwords.

## Optimizing File Size

While qpdf’s primary function is not to optimize the size of PDF files, there are a number of things you can do to make files smaller. Note that qpdf will not resample images or make optimizations that modify content with the exception of possibly recompressing images using DCT (JPEG) compression.

The following options will give you the smallest files that qpdf can generate:

- `--compress-streams=y`: make sure streams are compressed (see)
- `--decode-level=generalized`: apply any non-specialized filters (see )
- : uncompress and recompress streams that are already compressed with zlib (flate) compression
- `--compression-level=9`: use the highest possible compression level (see and )
- : replace non-JPEG images with JPEG if doing so reduces their size. Not all types of images are supported, but qpdf will only keep the result if it is supported and reduces the size. Images are not resampled, but bear in mind that JPEG is lossy, so images may have artifacts. These are not usually noticeable to the casual observer.
- `--object-streams=generate`: generate object streams, which means that more of the PDF file’s structural content will be compressed (see )

## Zopfli Compression Algorithm

If qpdf is built with [zopfli](https://github.com/google/zopfli) support (see [Building with zopfli support](https://qpdf.readthedocs.io/en/stable/installation.html#build-zopfli) ), you can have qpdf use the Zopfli Compression Algorithm in place of zlib. In this mode, qpdf is *much slower*, but produces slightly smaller compressed output. (According to their documentation, zopfli is about 100 times slower than zlib and produces output that’s about 5% better than the best compression available with other libraries.) For this to be useful, you should run qpdf with options to recompress compressed streams. See for tips. In order to use zopfli, in addition to building with zopfli support, you must set the `QPDF_ZOPFLI` environment variable to some value other than `disabled`. Note that has no effect when zopfli is in use, since zopfli always optimizes for size over everything else.

Here are the supported values for `QPDF_ZOPFLI`:

| `disabled` or unset | do not use zopfli even if available |
| --- | --- |
| `silent` | use zopfli if available; otherwise silently fall back to zlib |
| `force` | use zopfli if available; fail with an error if not available |
| any other value | use zopfli if available; otherwise issue a warning and fall back to zlib |

Note that the warning and error behavior are managed in `QPDFJob` and affect the `qpdf` executable. For code that directly uses the qpdf library, the behavior is that zopfli is enabled with any value other than `disabled` but silently falls back to zlib. If you want your application to behave the same as the `qpdf` executable with respect to zopfli, you can call `Pl_Flate::zopfli_check_env()`. See its documentation in the `qpdf/Pl_Flate.hh` header file.