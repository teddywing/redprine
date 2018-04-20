Redprine
========

Automatically update Redmine issues based on open pull requests.

After opening a pull request, you might need to update the corresponding Redmine
ticket with new status information. This program does that work automatically.

The script should be run through a service manager like `cron` or `launchd`. At
each run, it will fetch all open pull requests for the given project, and update
the corresponding Redmine issue of each new pull request.


## Usage
Several parameters are required to configure the script to enable it to
communicate with both GitHub and Redmine.

Both a [GitHub personal access token][1] and Redmine API key
(`https://redmine.example.com/my/account`) are needed.

Here’s an example script with all required arguments:

	#!/bin/sh

	redprine \
		--github-token 'GITHUB_TOKEN' \
		--github-username 'teddywing' \
		--github-owner-repo 'teddywing/redprine' \
		--redmine-base-url 'https://redmine.eaglejump.co' \
		--redmine-token 'REDMINE_TOKEN' \
		--redmine-json-params 'echo "
			{
				\"issue\": {
					\"status_id\": 4,
					\"done_ratio\": 100,
					\"custom_field_values\": {
						\"1\": \"$0\"
					}
				}
			}
		"'

In `--redmine-json-params`, enter a string containing a Bash command (it will be
executed with `bash -c`). A single argument is passed in `$0` containing the URL
of the issue’s pull request. The format must match Redmine’s JSON parameter for
the [issue update API endpoint][3].

Pull request branches are assumed to start with a four-digit number followed by
a hyphen (e.g. `1234-`), where the number is a Redmine issue ID.


## Install
Requirements:

* `curl`
* [`jq`][2]

Download and copy the [Redprine script][4] into your `PATH`.


## License
Copyright © 2018 Teddy Wing. Licensed under the GNU GPLv3+ (see the included
COPYING file).


[1]: https://github.com/settings/tokens
[2]: https://stedolan.github.io/jq/
[3]: https://www.redmine.org/projects/redmine/wiki/Rest_Issues#Updating-an-issue
[4]: https://raw.githubusercontent.com/teddywing/redprine/master/redprine
