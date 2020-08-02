# vidcut

Cut down a video file to just the desired segments using ffmpeg.  I use this tool to trim the fat from TV recordings from Plex's DVR feature.

The primary switch is...
- `--keep` to specify the Start and End times for segments you wish to _keep_.  Multiple `--keep` segments may be specified.

The default behavior is to replace the input file with the trimmed down version, while the original input file is retained with `-save` added to its name.
This may be modified with the `--out` and `--no-save` switches.

## Usage
```
$ ./vidcut -h
usage: vidcut [-h] [-o OUT] [--no-save] [--no-cleanup] [-v] [-V] -k Time Time
              Infile

Video file slicer.

One or more --keep's specify segments of the input file to retain in the output file.
The --keep start/end times (in Sec or H:M:S) are relative to the start of the input file.
EG:
    --keep 10 90 --keep 5:20 5:40
    retains 80s between 10s and 90s, and then 20s between 5min 20s to 5min 40s.

An end time of 0 specifies to keep to the end of the input file.  Otherwise, the
end time must be greater than the start time.  Keep segments may be in any time order and
may be overlapping, although the results may not be useful.

By default, the input file is renamed with '-save' and the output is written in its place.
If --out is specified the input file is left in-place.
The input file is simply replaced if --no-save is specified and --out is not specified.

Note: Intermediate files are written in the script directory, as specified by WORKDIR, and
normally cleaned up (unless --no-cleanup).

positional arguments:
  Infile                The input file.

optional arguments:
  -h, --help            show this help message and exit
  -o OUT, --out OUT     The output file (the default is to replace Infile).
  --no-save             Used with --out, delete Infile after processing (the default is to rename Infile with '-save').
  --no-cleanup          Don't clean up intermediate files for multi-segment usage.
  -v, --verbose         Print status and activity messages.
  -V, --version         Return version number and exit.

required arguments:
  -k Time Time, --keep Time Time
                        Keep segment START and END times.  May be specified one or more times.
```

## Setup and Usage notes
- Supported on Python3 only.  Developed on Centos 7.8 with Python 3.6.8.  Tested/supported on Windows 10 with Python 3.8.0.
- Depends on ffmpeg being within the user's path env var.  An alternate ffmpeg executable may be specified via the `FFMPEG` constant in the code.
- The temporary files working directory WORKDIR may be configured in the script.  The default is the script's directory.
- Note that ffmpeg _copy_ is used so that the original quality is retained - no transcoding.  

## Known issues:
- No adjustments are made to the output file, such as PTS adjustments.  The segments are simply concatenated together.  There may be some errors in the output that have not yet been found.

## Version history

- 200802 v0.1  Added configurable WORKDIR, Supporting Windows
- 200801 v0.0  New
