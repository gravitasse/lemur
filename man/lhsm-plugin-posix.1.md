% LHSMD (1) User Manaual
% Intel Corporation
% REPLACE_DATE

# NAME

lhsm-plugin-posix - Lhsmd plugin for POSIX archives

# DESCRIPTION

`lhsm-plugin-posix` is a data mover that supports archiving data to a POSIX file system. It is not intended
to be run directly, and should only be run by `lhsmd`.  It is configured using the
configuration file.

# GENERAL USAGE

The default location for the mover configuration file is `/etc/lhsmd/lhsm-plugin-posix`.
These are the configuration options available.

`num_threads`
:     The maximum number of concurrent copy requests the plugin will allow.

`archive`
:    Each `archive` section configures an archive endpoint that will be registered with the agent
     and corresponds with a Lustre Archive ID. It is important that each Archive ID be used with the
     same endpoint on each data mover newUploader

     `id`
     :     The ID associated with this archive.

     `root`
     :     The base directory of the archive. Must be accessible on the mover node.

     `compression`
     :     If set to "on", all data will be compressed when written to the backend. Compressed
           data objects will be automatically uncompressed during restore even if this option has been
           subsequently disabled.
           If set to "auto", then a small portion of each file will be compressed to determine
           if the file should be compressed or not. If the initial check yields better than 30%
           compression, the the file will be compressed. This still an experimental feature
           and will likely need further refinement and optimization.

     `checksums`
     :    By default, data checksums are created when a file is archived and validated on restore.
          These options can be used to disable checksums entirely or just disable restore validation (useful
          if checksums have been corrupted or lost).

          `disabled`
          :     This inhibits creation of file checksums.

          `disable_compare_on_restore`
          :    This prevents checking file checksums on restore.


# EXAMPLES

A sample S3 plugin configuration with one archive:

        num_threads = 8

        archive "posix-test" {
           id = 1
           root = "/tmp/archive"
           compression = "off"
           checksums {
                disabled = false
           }
        }

# SEE ALSO

`lhsmd` (1)
