# hfs2dfxml
Utility to parse hfsutils output and produce DFXML for HFS-formatted disk images

## Caveat
This script is still in development. It is recommended that you check your results against another tool. If you encounter an error or unexpected results, please file an issue on GitHub, or use the contact information below.

## System requirements and setup
* `hfsutils` (http://www.mars.org/home/rob/proj/hfs; or installed via your distribution's package manager)
* `python3-magic`
* `xmllint` for validation (in tests) and pretty-printing DFXML output
* DFXML Schema (http://github.org/dfxml-working-group/dfxml-schema) for testing and validation of results
* `tests/hfs2dfxml_tests.py` - Specify path for image file in script to run tests
* `git`
* `homebrew` - (https://brew.sh/) macOS requirement; used to set-up hfsutils and libmagic

## BitCurator and macOS installation directions
The following installation/configuration steps require the Terminal. (For Ubuntu, this was last tested on BitCurator 1.7.98.)

* Test dependencies
  * `which xmllint` - Output should be `/usr/bin/xmllint`
  * `which git` - Output should be `/usr/bin/git`
  * `which hmount` - Output should be `/usr/local/bin/hmount` (no output means hfsutils is not installed)
* Install hfsutils (only if `which hmount` fails; see above)
  * BitCurator 
    * `sudo apt-get install hfsutils` (to install hfsutils)
    * `which hmount` - Output should be `/usr/bin/hmount` (no output means hfsutils is not installed)
  * macOS 
    * `brew install hfsutils` 
* Get python magic
  * BitCurator
    * `sudo apt-get install python3-magic` (or `pip3 install python-magic`; see below)
  * macOS - python-magic and libmagic
    * `pip3 install python-magic`
    * `brew install libmagic`
* Download hfs2dfxml and set up DFXML libraries and schema (Note: for simplicity, this assumes you'd like to install it to the Desktop. You can certainly install this elsewhere! The directions are essentially the same, save for the installation location.)
  * `cd ~/Desktop`
  * `git clone https://github.com/cul-it/hfs2dfxml`
  * `cd ~/Desktop/hfs2dfxml`
  * `git submodule update --init --recursive`
  * `cd ../tests`
  * `git clone https://github.com/dfxml-working-group/dfxml_schema`

## How to use
To generate DFXML for an HFS-formatted volume:
* `cd ~/Desktop/hfs2dfxml/hfs2dfxml`
* `python3 hfs2dfxml.py [HFS volume] [output file]`

Note: `[output file]` must not already exist.

Optional parameters: 

`-s --strict`: Use `xmllint` and generate easy to read and parsed XML

`-d, --delimiter [classic, macosx, osx, companion]`
* `classic`: Output paths with a colon delimiter; reports resource forks as `filename:rsrc` (Default)
* `macosx`: Output paths with a slash delimiter; reports resource forks as `filename/rsrc`
* `osx`: Output paths with a slash delimiter; reports resource forks as `filename/..namedfork/rsrc`
* `companion`: Output paths with a slash delimiter; reports resource forks as `._filename`

Optionally, place hfs2dfxml in your Python path and import it in your own code to call `hfs_volobj`. This function returns a standalone DFXML Volume object.

## Things that commonly go wrong
If you encounter an error with ascii codecs, ensure you're running the code with python3.

If you encounter the following error, "magic module object has no attribte 'open'", this means you're experiencing the effects of two python modules with the same name. I'm sorry. Please test the `magic` branch and let me know if it works for you:
  * `cd ~/Desktop/hfs2dfxml`
  * `git pull`
  * `git checkout magic`
  * Then follow the same use instructions above.

## Known limitations (and implied to do list)
* HFS namespace is projected and not yet officially part of the DFXML schema. See: https://github.com/dfxml-working-group/dfxml_schema/issues/23
* Byte runs not reported for fileobjects
* Timestamps only include day/month/year and not specific time
* Tested on CD-ROM disc image of HFS volume only; submissions of additional HFS volumes to do further testing will be happily accepted.


If you encounter a bug or issue not listed above, please feel free to file an issue in GitHub. There is a DEBUG flag at the top of the script which can be set to `True` and that will produce a logfile named `DEBUG_hfs2dfxml.txt` to assist in debugging, or can be used directly in the `debug_raw_hfs.py` file.

### Contact
dd388 AT cornell DOT edu

### Thank you
* Alex Nelson for DFXML guidance and help prioritizing code features.
* Kate Tasker for testing various HFS disk images and supplying data for debugging.
* Jessica Venlet for adding macOS instructions to this Readme.
