# DDNS Route53

Keeps Route 53 A records pointed at the current public IP of the home network.
Each run fetches the public IP from `checkip.amazonaws.com`, finds the hosted
zone for each domain under the given AWS profile, and upserts the A record when
it differs. It prints only when it changes a record or fails, so the log stays
near empty on an SD card.

## Usage

```shell
venv/bin/python ddns.py <aws-profile> <domain> [domain ...]
```

The profile must exist in `~/.aws/credentials` and needs
`route53:ListHostedZones`, `route53:ListResourceRecordSets` and
`route53:ChangeResourceRecordSets` on the target zones.

## Deploy

`bin/deploy` syncs the repo to `pi@192.168.1.201:~/Development/ddns-route53` and
installs dependencies into a venv there. Pass another `user@host` to deploy
elsewhere.

## Schedule

One crontab line per AWS account:

```shell
*/15 * * * * cd $HOME/Development/ddns-route53 && venv/bin/python ddns.py ddns pimodo.charleybyrne.com eagata.com formaweave.com >> $HOME/ddns.log 2>&1
*/15 * * * * cd $HOME/Development/ddns-route53 && venv/bin/python ddns.py kbpc ballyconnellns.ie >> $HOME/ddns.log 2>&1
```
