# Google Drive Arq Backup Cleaner
This script deletes all [Arq Backup](http://www.arqbackup.com) files

**WARNING** it'll deletes all Google Drive binary (unknown) files which filename is 40chars so _run it at your own risk_

## Dependencies
To use the Python script directly
* Python 3.5+
* package *google-api-python-client*  
run `pip install --upgrade google-api-python-client` to install

### Google authorization
The first time you run `cleaner`, you will be prompted with a Google authorization page asking you for permission to view and manage your Google Drive files.
Once authorized, a credential file will be saved in `.credentials\google-drive-trash-cleaner.json` under your home directory (`%UserProfile%` on Windows).
You don't need to manually authorize `cleaner` again until you delete this credential file or revoke permission on your Google [account](https://myaccount.google.com/permissions "Apps connected to your account") page.  
You can specify a custom location for the credential file by using the command line option `--credfile`. This is helpful if you're using multiple Google accounts with `cleaner`.

### `page_token` file
`cleaner` finds out when your files were trashed by scanning through your Google Drive activity history.
On first run, it must start from the very beginning to ensure no files are missed, so it might take some time.
After first run, `cleaner` saves a file named `page_token` in its own parent folder.
This file contains a single number indicating an appropriate starting position in your Google Drive activity history for future scans,
so they can be much faster than the first one. Each run of `cleaner` updates `page_token` as appropriate.  
You can specify a custom location or name for the `page_token` file by using the command line option `--ptokenfile`.

### More options
More command line options are available. You can read about them by running `cleaner --help`.
```
usage: cleaner [-h] [-a] [-v] [-d #] [-q] [-t SECS] [-m] [--noprogress]
               [--fullpath] [--logfile PATH] [--ptokenfile PATH]
               [--credfile PATH]

optional arguments:
  -h, --help            show this help message and exit
  -a, --auto            Automatically delete older trashed files in Google
                        Drive without prompting user for confirmation
  -v, --view            Only view which files are to be deleted without
                        deleting them
  -d #, --days #        Number of days files can remain in Google Drive trash
                        before being deleted. Default is 30
  -q, --quiet           Quiet mode. Only show file count.
  -t SECS, --timeout SECS
                        Specify timeout period in seconds. Default is 300
  -m, --mydriveonly     Only delete files in the 'My Drive' hierarchy,
                        excluding those in 'Computers' etc.
  --noprogress          Don't show scanning progress. Useful when directing
                        output to files.
  --fullpath            Show full path to files. May be slow for a large
                        number of files. NOTE: the path shown is the 'current'
                        path, may be different from the original path (when
                        trashing) if the original parent folder has moved.
  --logfile PATH        Path to log file. Default is no logs
```

### Credit
The idea for the script's working mechanism is borrowed from
[this Stack Overflow question](https://stackoverflow.com/questions/34803290/how-to-retrieve-a-recent-list-of-trashed-files-using-google-drive-api).

Based on http://github.com/cfbao/google-drive-trash-cleaner
