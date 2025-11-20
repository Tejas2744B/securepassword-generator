A flexible and scriptable password generator which generates strong passphrases, inspired by XKCD 936:

$ xkcdpass
> correct horse battery staple
<img width="740" height="601" alt="687474703a2f2f696d67732e786b63642e636f6d2f636f6d6963732f70617373776f72645f737472656e6774682e706e67" src="https://github.com/user-attachments/assets/ae427053-a230-43b8-95ed-ca28f7bb6aa4" />
Install
xkcdpass can be easily installed using pip:

pip install xkcdpass
or manually:

python setup.py install
Requirements
Python 2 (version 2.7 or later), or Python 3 (version 3.4 or later). Running module unit tests on Python 2 requires mock to be installed.

Running xkcdpass
xkcdpass can be called with no arguments:

$ xkcdpass
> pinball previous deprive militancy bereaved numeric
which returns a single password, using the default dictionary and default settings. Or you can mix whatever arguments you want:

$ xkcdpass --count=5 --acrostic='chaos' --delimiter='|' --min=5 --max=6 --valid-chars='[a-z]'
> collar|highly|asset|ovoid|sultan
> caper|hangup|addle|oboist|scroll
> couple|honcho|abbot|obtain|simple
> cutler|hotly|aortae|outset|stool
> cradle|helot|axial|ordure|shale
which returns

--count=5 5 passwords to choose from
--acrostic='chaos' the first letters of which spell 'chaos'
--delimiter='|' joined using '|'
--min=5 --max=6 with words between 5 and 6 characters long
--valid-chars='[a-z]' using only lower-case letters (via regex).
A concise overview of the available xkcdpass options can be accessed via:

xkcdpass --help

Usage: xkcdpass [options]

Options:
    -h, --help
                                show this help message and exit
    -w WORDFILE, --wordfile=WORDFILE
                                Specify that the file WORDFILE contains the list of
                                valid words from which to generate passphrases. Multiple
                                wordfiles can be provided, separated by commas.
                                Provided wordfiles: eff-long (default), eff-short,
                                eff-special, legacy, spa-mich (Spanish), fin-kotus (Finnish)
                                ita-wiki (Italian), ger-anlx, ger-long, ger-short (German), nor-nb (Norwegian),
                                fr-freelang (French), pt-ipublicis / pt-l33t-ipublicis (Portuguese)
                                swe-short (Swedish)
    --min=MIN_LENGTH
                                Minimum length of words to make password
    --max=MAX_LENGTH
                                Maximum length of words to make password
    -n NUMWORDS, --numwords=NUMWORDS
                                Number of words to make password
    -i, --interactive
                                Interactively select a password
    -v VALID_CHARS, --valid-chars=VALID_CHARS
                                Valid chars, using regexp style (e.g. '[a-z]')
    -V, --verbose
                                Report various metrics for given options, including word list entropy
    -a ACROSTIC, --acrostic=ACROSTIC
                                Acrostic to constrain word choices
    -c COUNT, --count=COUNT
                                number of passwords to generate
    -d DELIM, --delimiter=DELIM
                                separator character between words
    -R, --random-delimiters
                                use randomised delimiters
    -D DELIMITERS, --valid-delimiters=DELIMETERS
                                delimeters to choose from, used with -
    -s SEP, --separator SEP
                                Separate generated passphrases with SEP.
    -C CASE, --case CASE
                                Choose the method for setting the case of each word in
                                the passphrase. Choices: ['alternating', 'upper',
                                'lower', 'random', 'capitalize', 'as-is'] (default: 'lower').
    --allow-weak-rng
                                 Allow fallback to weak RNG if the system does not
                                support cryptographically secure RNG. Only use this if
                                you know what you are doing.
  Using xkcdpass as an imported module
The built-in functionality of xkcdpass can be extended by importing the module into python scripts. An example of this usage is provided in example_import.py, which randomly capitalises the letters in a generated password. example_json.py demonstrates integration of xkcdpass into a Django project, generating password suggestions as JSON to be consumed by a Javascript front-end.

A simple use of import:

from xkcdpass import xkcd_password as xp

# create a wordlist from the default wordfile
# use words between 5 and 8 letters long
wordfile = xp.locate_wordfile()
mywords = xp.generate_wordlist(wordfile=wordfile, min_length=5, max_length=8)

# create a password with the acrostic "face"
print(xp.generate_xkcdpassword(mywords, acrostic="face"))
When used as an imported module, generate_wordlist() takes the following args (defaults shown):

wordfile=None,
min_length=5,
max_length=9,
valid_chars='.'
While generate_xkcdpassword() takes:

wordlist,
numwords=6,
interactive=False,
acrostic=False,
delimiter=" "
Insecure random number generators
xkcdpass uses crytographically strong random number generators where possible (provided by random.SystemRandom() on most modern operating systems). From version 1.7.0 falling back to an insecure RNG must be explicitly enabled, either by using a new command line variable before running the script:

xkcdpass --allow-weak-rng
or setting the appropriate environment variable:

export XKCDPASS_ALLOW_WEAKRNG=1
