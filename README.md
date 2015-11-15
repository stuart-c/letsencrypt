# LET'S ENCRYPT WRAPPER

## REQUIREMENTS

* Intel or ARM machine
* Docker
* Apache or nginx webserver (Can be containerised)
* Optional **openssl** command

## INSTALLATION

1. Copy the **letsencrypt** file to somewhere in your PATH
2. Configure your webserver to serve **/.well-known** from your webroot (default **/var/www/html**)

   For Apache you can just copy the file **apache/letsencrypt.conf** into **/etc/apache2/conf-enabled**

## ISSUING NEW CERTIFICATES

Run `letsencrypt www.example.com example.com` to issue a certificate for www.example.com which also includes example.com as one of the Subject Alternative Names (SANs).

You will need to supply your email address, either via a [configuration file](#configuration-files) or add `--email webmaster@example.com` to your command line.

Your webserver (Apache or nginx, on the host or within a Docker container) can be configured to automatically be restarted or reloaded upon successful certificate issuance. Add `--reload` or `--restart` to your command line or add the *afterupdate* [configuration file](#configuration-files) parameter.

If the webserver is containerised, add `--container <container>` to your command line or add the *container* [configuration file](#configuration-files) parameter.

## AUTOMATIC CERTIFICATE RENEWAL

Running `letsencrypt --renew` will check all issued certificates and renew all those which are approaching expiry. By default this is 30 days before expiry, but it can be changed via the `--renewal-days <days>` command line option or the *renewaldays* [configuration file](#configuration-files) parameter.

Note: You will need to run `letsencrypt` as user who can read the certificate storage directory (default **/etc/letsencrypt**). This usually means you want to run with `sudo` for renewals.

To fully automate certificate renewal add the following to your root crontab:

> `@daily letsencrypt --renew`

## CONFIGURATION FILES

In addition to command line options, configuration files can be used to set parameters. The files **/etc/letsencrypt.conf** and **~/.letsencrypt.conf** are always checked first, with additional files to check specified via the `--config letsencrypt.conf` command line option. This option can be given multiple times. Configuration files specified on the command line are checked after the two built in files, in the order specified. Where a configuration parameter is set in multiple files the last file checked takes precedence. Command line options always override configuration file parameters.

Configuration files are plain text, with lines structured as:

> `name = value`

Any content after a **#** character is taken to be a comment and is ignored. Blank lines are also ignored.

## COMMAND LINE & CONFIGURATION OPTIONS

| Command line option      | Config file parameter | Description                                                 | Default              |
|--------------------------|-----------------------|-------------------------------------------------------------|----------------------|
| -h, --help               |                       | Show help message                                           |                      |
| --quiet, -q              |                       | Show less information                                       |                      |
| --verbose, -v            |                       | Show more information                                       |                      |
| --debug                  |                       | Show debugging information                                  |                      |
| --config FILE, -c FILE   |                       | Configuration file to load. Can be specified multiple times |                      |
| --server URL             | server                | Which ACME server to use                                    | test                 |
| --live                   | server = live         | Use Let's Encrypt live ACME server                          |                      |
| --test                   | server = test         | Use Let's Encrypt test ACME server                          |                      |
| --webserver TYPE         | webserver             | Webserver type                                              | auto                 |
| --auto                   | webserver = auto      | Autodetect webserver                                        |                      |
| --apache                 | webserver = apache    | Webserver is Apache                                         |                      |
| --nginx                  | webserver = nginx     | Webserver is nginx                                          |                      |
| --container NAME         | container             | Webserver Docker container name                             |                      |
| --after-update ACTION    | afterupdate           | What to do after an update                                  | nothing              |
| --keep-running           | afterupdate = nothing | Do nothing after an update                                  |                      |
| --reload                 | afterupdate = reload  | Reload webserver after an update                            |                      |
| --restart                | afterupdate = restart | Restart webserver after an update                           |                      |
| --email EMAIL            | email                 | Email address to send to ACME server                        |                      |
| --webroot DIRECTORY      | webroot               | Webroot directory                                           | /var/www/html        |
| --certificates DIRECTORY | certificates          | Certificate storage directory                               | /etc/letsencrypt     |
| --logs DIRECTORY         | logs                  | Logging directory                                           | /var/log/letsencrypt |
| --openssl COMMAND        | openssl               | OpenSSL command to use                                      | openssl              |
| --docker COMMAND         | docker                | Docker command to use                                       | docker               |
| --apachectl COMMAND      | apachectl             | apachectl command to use                                    | apachectl            |
| --nginxcmd COMMAND       | nginxcmd              | nginx command to use                                        | nginx                |
| --letsencrypt-image NAME | letsencryptimage      | Lets Encrypt Docker image name                              | auto                 |
| --openssl-image NAME     | opensslimage          | OpenSSL Docker image name                                   | auto                 |
| --issue                  |                       | Issue new certificate                                       |                      |
| --renew                  |                       | Check for certificates to renew                             |                      |
| --renewal-days DAYS      | renewaldays           | Days before expiry to renew                                 | 30                   |

## CONTRIBUTING

Please report any issues or suggestions via GitHub at <https://github.com/stuart-c/letsencrypt>

I can be contacted at <stuart.clark@Jahingo.com>

## LICENSE

> Copyright (C) 2015, Stuart Clark; stuart.clark@Jahingo.com
>
> This program is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation; either version 2 of the License, or (at your option) any later version.
>
> This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License for more details.
>
> You should have received a copy of the GNU General Public License along with this program; if not, write to the Free Software Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.

