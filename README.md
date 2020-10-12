[![Build](https://gitlab.com/hectorm/hblock/badges/master/build.svg)](https://gitlab.com/hectorm/hblock/pipelines)
[![Website](https://img.shields.io/website/https/hblock.molinero.dev.svg)](https://hblock.molinero.dev)
[![License](https://img.shields.io/github/license/hectorm/hblock.svg)](LICENSE.md)

***

<p align="center">
  <a href="https://hblock.molinero.dev/">
    <img src="https://hectorm.github.io/hblock/resources/logo/logotype.svg" width="320" height="100">
  </a>
</p>

Improve your security and privacy by blocking ads, tracking and malware domains.

## Table of contents

* [What is this for?](#what-is-this-for)
* [Is it safe to use?](#is-it-safe-to-use)
* [Nightly builds](#nightly-builds)
* [Installation](#installation)
  * [Manual](#manual)
  * [Gentoo](#gentoo)
  * [NPX](#npx)
* [Usage](#usage)
  * [Script arguments](#script-arguments)
  * [Preserve content](#preserve-content)
  * [Run preview](#run-preview)
* [Sources](#sources)
* [Disclaimer](#disclaimer)
* [License](#license)

## What is this for?

This POSIX-compliant shell script, designed for Unix-like systems, gets a list of domains that serve ads, tracking scripts and malware from
[multiple sources](#sources) and creates a [hosts file](https://en.wikipedia.org/wiki/Hosts_(file)) (alternative formats are also supported) that
prevents your system from connecting to them.

## Is it safe to use?

Absolutely, this script selects only the domain names for each source, so if a domain name is redirected to a rogue server your system will not be
affected. In the worst scenario you can lose access to a legitimate domain name due a false positive, but you can reverse it by adding that domain to
the allowlist.

## Nightly builds

Nightly builds of the hosts file are available here among other formats.

https://hblock.molinero.dev

## Installation

### Manual

```sh
curl -o /tmp/hblock 'https://raw.githubusercontent.com/hectorm/hblock/v2.1.7/hblock' \
  && echo 'e357fc9439d7b79036c7939f976aab9cbd25878c4651e5a757e30ff96452edc2  /tmp/hblock' | shasum -c \
  && sudo mv /tmp/hblock /usr/local/bin/hblock \
  && sudo chown root:root /usr/local/bin/hblock \
  && sudo chmod 755 /usr/local/bin/hblock
```

**Note:** you can use [this systemd timer](resources/systemd/README.md) to regularly update the hosts file for new additions.

### Gentoo

 * Add the src_prepare overlay with the help of the [official repository](https://gitlab.com/src_prepare/src_prepare-overlay#adding-the-overlay).
 * Unmask the `net-firewall/hblock` package with the help of the [Gentoo wiki](https://wiki.gentoo.org/wiki/Knowledge_Base:Unmasking_a_package).
 * Run:

   ```sh
   emerge --verbose net-firewall/hblock
   ```

### NPX

It is possible to use [NPX](https://www.npmjs.com/package/npx) to run hBlock without installation.

```sh
npx hblock
```

## Usage

#### Script arguments

```
 -O, --output <FILE>
        Output file location.
         * Environment variable: HBLOCK_OUTPUT_FILE
         * Default value: /etc/hosts
 -H, --header <FILE>
        File to be included at the beginning of the output file.
        If the default file does not exist or equals "builtin" the built-in
        value is used instead.
         * Environment variable: HBLOCK_HEADER_FILE
         * Default value: /etc/hblock/header
 -F, --footer <FILE>
        File to be included at the end of the output file.
        If the default file does not exist or equals "builtin" the built-in
        value is used instead.
         * Environment variable: HBLOCK_FOOTER_FILE
         * Default value: /etc/hblock/footer
 -S, --sources <FILE>
        File with line separated URLs used to generate the blocklist.
        If the default file does not exist or equals "builtin" the built-in
        value is used instead.
         * Environment variable: HBLOCK_SOURCES_FILE
         * Default value: /etc/hblock/sources.list
 -A, --allowlist <FILE>
        File with line separated entries to be removed from the blocklist.
        If the default file does not exist or equals "builtin" the built-in
        value is used instead.
         * Environment variable: HBLOCK_ALLOWLIST_FILE
         * Default value: /etc/hblock/allow.list
 -D, --denylist <FILE>
        File with line separated entries to be added to the blocklist.
        If the default file does not exist or equals "builtin" the built-in
        value is used instead.
         * Environment variable: HBLOCK_DENYLIST_FILE
         * Default value: /etc/hblock/deny.list
 -R, --redirection <REDIRECTION>
        Redirection for all entries in the blocklist.
         * Environment variable: HBLOCK_REDIRECTION
         * Default value: 0.0.0.0
 -T, --template <TEMPLATE>
        POSIX BREs replacement applied to each entry.
        Capturing group backreferences: \1 = <DOMAIN>, \2 = <REDIRECTION>
         * Environment variable: HBLOCK_TEMPLATE
         * Default value: \2 \1
 -C, --comment <COMMENT>
        Character used for comments.
         * Environment variable: HBLOCK_COMMENT
         * Default value: #
 -l, --[no-]lenient
        Match all entries from sources regardless of their IP, instead
        of 0.0.0.0, 127.0.0.1, ::, ::1 or nothing.
         * Environment variable: HBLOCK_LENIENT
         * Default value: false
 -r, --[no-]regex
        Use POSIX BREs in the allowlist instead of fixed strings.
         * Environment variable: HBLOCK_REGEX
         * Default value: false
 -c, --[no-]continue
        Do not abort if a download error occurs.
         * Environment variable: HBLOCK_CONTINUE
         * Default value: false
 -q, --[no-]quiet
        Suppress non-error messages.
         * Environment variable: HBLOCK_QUIET
         * Default value: false
 -x, --color <auto|true|false>
        Colorize the output.
         * Environment variable: HBLOCK_COLOR
         * Default value: auto
 -v, --version
        Show version number and quit.
 -h, --help
        Show this help and quit.
```

#### Run preview

[![asciicast](https://asciinema.org/a/U0eSfh04zgf3zR9F2hKAZawbm.svg)](https://asciinema.org/a/U0eSfh04zgf3zR9F2hKAZawbm)

## Sources

| Name                                    | Primary                                          | Mirror                                           |
|-----------------------------------------|:------------------------------------------------:|:------------------------------------------------:|
| adaway.org                              | [URL][source-adaway.org]                         | [URL][mirror-adaway.org]                         |
| AdBlock NoCoin List                     | [URL][source-adblock-nocoin-list]                | [URL][mirror-adblock-nocoin-list]                |
| AdGuard - Simplified                    | [URL][source-adguard-simplified]                 | [URL][mirror-adguard-simplified]                 |
| AntiPopads                              | [URL][source-antipopads]                         | [URL][mirror-antipopads]                         |
| disconnect.me - Ad                      | [URL][source-disconnect.me-ad]                   | [URL][mirror-disconnect.me-ad]                   |
| disconnect.me - Malvertising            | [URL][source-disconnect.me-malvertising]         | [URL][mirror-disconnect.me-malvertising]         |
| disconnect.me - Malware                 | [URL][source-disconnect.me-malware]              | [URL][mirror-disconnect.me-malware]              |
| disconnect.me - Tracking                | [URL][source-disconnect.me-tracking]             | [URL][mirror-disconnect.me-tracking]             |
| DShield.org - High                      | [URL][source-dshield.org-high]                   | [URL][mirror-dshield.org-high]                   |
| EasyList                                | [URL][source-easylist]                           | [URL][mirror-easylist]                           |
| EasyPrivacy                             | [URL][source-easyprivacy]                        | [URL][mirror-easyprivacy]                        |
| ETH Phishing Detect                     | [URL][source-eth-phishing-detect]                | [URL][mirror-eth-phishing-detect]                |
| FadeMind - add.2o7Net                   | [URL][source-fademind-add.2o7net]                | [URL][mirror-fademind-add.2o7net]                |
| FadeMind - add.Dead                     | [URL][source-fademind-add.dead]                  | [URL][mirror-fademind-add.dead]                  |
| FadeMind - add.Risk                     | [URL][source-fademind-add.risk]                  | [URL][mirror-fademind-add.risk]                  |
| FadeMind - add.Spam                     | [URL][source-fademind-add.spam]                  | [URL][mirror-fademind-add.spam]                  |
| Geoffrey Frogeye - First-party trackers | [URL][source-gfrogeye-firstparty-trackers]       | [URL][mirror-gfrogeye-firstparty-trackers]       |
| hostsVN                                 | [URL][source-hostsvn]                            | [URL][mirror-hostsvn]                            |
| KADhosts                                | [URL][source-kadhosts]                           | [URL][mirror-kadhosts]                           |
| lightswitch05 - Ads & Tracking          | [URL][source-lightswitch05-ads-and-tracking]     | [URL][mirror-lightswitch05-ads-and-tracking]     |
| malwaredomainlist.com                   | [URL][source-malwaredomainlist.com]              | [URL][mirror-malwaredomainlist.com]              |
| malwaredomains.com - Immortal domains   | [URL][source-malwaredomains.com-immortaldomains] | [URL][mirror-malwaredomains.com-immortaldomains] |
| malwaredomains.com - Just domains       | [URL][source-malwaredomains.com-justdomains]     | [URL][mirror-malwaredomains.com-justdomains]     |
| matomo.org - Spammers                   | [URL][source-matomo.org-spammers]                | [URL][mirror-matomo.org-spammers]                |
| mitchellkrogza - Badd-Boyz-Hosts        | [URL][source-mitchellkrogza-badd-boyz-hosts]     | [URL][mirror-mitchellkrogza-badd-boyz-hosts]     |
| pgl.yoyo.org                            | [URL][source-pgl.yoyo.org]                       | [URL][mirror-pgl.yoyo.org]                       |
| Phishing Army                           | [URL][source-phishing.army]                      | [URL][mirror-phishing.army]                      |
| someonewhocares.org                     | [URL][source-someonewhocares.org]                | [URL][mirror-someonewhocares.org]                |
| spam404.com                             | [URL][source-spam404.com]                        | [URL][mirror-spam404.com]                        |
| StevenBlack                             | [URL][source-stevenblack]                        | [URL][mirror-stevenblack]                        |
| uBlock                                  | [URL][source-ublock]                             | [URL][mirror-ublock]                             |
| uBlock - Badware                        | [URL][source-ublock-badware]                     | [URL][mirror-ublock-badware]                     |
| uBlock - Privacy                        | [URL][source-ublock-privacy]                     | [URL][mirror-ublock-privacy]                     |
| winhelp2002.mvps.org                    | [URL][source-winhelp2002.mvps.org]               | [URL][mirror-winhelp2002.mvps.org]               |
| ZeroDot1 - CoinBlockerLists             | [URL][source-zerodot1-coinblockerlists-browser]  | [URL][mirror-zerodot1-coinblockerlists-browser]  |

[source-adaway.org]: https://adaway.org/hosts.txt
[mirror-adaway.org]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/adaway.org/list.txt
[source-adblock-nocoin-list]: https://raw.githubusercontent.com/hoshsadiq/adblock-nocoin-list/master/hosts.txt
[mirror-adblock-nocoin-list]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/adblock-nocoin-list/list.txt
[source-adguard-simplified]: https://filters.adtidy.org/extension/chromium/filters/15.txt
[mirror-adguard-simplified]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/adguard-simplified/list.txt
[source-antipopads]: https://raw.githubusercontent.com/Yhonay/antipopads/master/hosts
[mirror-antipopads]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/antipopads/list.txt
[source-disconnect.me-ad]: https://s3.amazonaws.com/lists.disconnect.me/simple_ad.txt
[mirror-disconnect.me-ad]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/disconnect.me-ad/list.txt
[source-disconnect.me-malvertising]: https://s3.amazonaws.com/lists.disconnect.me/simple_malvertising.txt
[mirror-disconnect.me-malvertising]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/disconnect.me-malvertising/list.txt
[source-disconnect.me-malware]: https://s3.amazonaws.com/lists.disconnect.me/simple_malware.txt
[mirror-disconnect.me-malware]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/disconnect.me-malware/list.txt
[source-disconnect.me-tracking]: https://s3.amazonaws.com/lists.disconnect.me/simple_tracking.txt
[mirror-disconnect.me-tracking]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/disconnect.me-tracking/list.txt
[source-dshield.org-high]: https://www.dshield.org/feeds/suspiciousdomains_High.txt
[mirror-dshield.org-high]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/dshield.org-high/list.txt
[source-easylist]: https://easylist.to/easylist/easylist.txt
[mirror-easylist]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/easylist/list.txt
[source-easyprivacy]: https://easylist.to/easylist/easyprivacy.txt
[mirror-easyprivacy]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/easyprivacy/list.txt
[source-eth-phishing-detect]: https://raw.githubusercontent.com/MetaMask/eth-phishing-detect/master/src/hosts.txt
[mirror-eth-phishing-detect]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/eth-phishing-detect/list.txt
[source-fademind-add.2o7net]: https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.2o7Net/hosts
[mirror-fademind-add.2o7net]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/fademind-add.2o7net/list.txt
[source-fademind-add.dead]: https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.Dead/hosts
[mirror-fademind-add.dead]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/fademind-add.dead/list.txt
[source-fademind-add.risk]: https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.Risk/hosts
[mirror-fademind-add.risk]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/fademind-add.risk/list.txt
[source-fademind-add.spam]: https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.Spam/hosts
[mirror-fademind-add.spam]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/fademind-add.spam/list.txt
[source-gfrogeye-firstparty-trackers]: https://hostfiles.frogeye.fr/firstparty-trackers.txt
[mirror-gfrogeye-firstparty-trackers]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/gfrogeye-firstparty-trackers/list.txt
[source-hostsvn]: https://raw.githubusercontent.com/bigdargon/hostsVN/master/option/hosts-VN
[mirror-hostsvn]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/hostsvn/list.txt
[source-kadhosts]: https://raw.githubusercontent.com/azet12/KADhosts/master/KADhosts.txt
[mirror-kadhosts]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/kadhosts/list.txt
[source-lightswitch05-ads-and-tracking]: https://www.github.developerdan.com/hosts/lists/ads-and-tracking-extended.txt
[mirror-lightswitch05-ads-and-tracking]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/lightswitch05-ads-and-tracking/list.txt
[source-malwaredomainlist.com]: https://www.malwaredomainlist.com/hostslist/hosts.txt
[mirror-malwaredomainlist.com]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/malwaredomainlist.com/list.txt
[source-malwaredomains.com-immortaldomains]: http://mirror1.malwaredomains.com/files/immortal_domains.txt
[mirror-malwaredomains.com-immortaldomains]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/malwaredomains.com-immortaldomains/list.txt
[source-malwaredomains.com-justdomains]: http://mirror1.malwaredomains.com/files/justdomains
[mirror-malwaredomains.com-justdomains]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/malwaredomains.com-justdomains/list.txt
[source-matomo.org-spammers]: https://raw.githubusercontent.com/matomo-org/referrer-spam-blacklist/master/spammers.txt
[mirror-matomo.org-spammers]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/matomo.org-spammers/list.txt
[source-mitchellkrogza-badd-boyz-hosts]: https://raw.githubusercontent.com/mitchellkrogza/Badd-Boyz-Hosts/master/hosts
[mirror-mitchellkrogza-badd-boyz-hosts]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/mitchellkrogza-badd-boyz-hosts/list.txt
[source-pgl.yoyo.org]: https://pgl.yoyo.org/adservers/serverlist.php?hostformat=nohtml&mimetype=plaintext
[mirror-pgl.yoyo.org]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/pgl.yoyo.org/list.txt
[source-phishing.army]: https://phishing.army/download/phishing_army_blocklist.txt
[mirror-phishing.army]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/phishing.army/list.txt
[source-someonewhocares.org]: http://someonewhocares.org/hosts/hosts
[mirror-someonewhocares.org]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/someonewhocares.org/list.txt
[source-spam404.com]: https://raw.githubusercontent.com/Spam404/lists/master/main-blacklist.txt
[mirror-spam404.com]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/spam404.com/list.txt
[source-stevenblack]: https://raw.githubusercontent.com/StevenBlack/hosts/master/data/StevenBlack/hosts
[mirror-stevenblack]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/stevenblack/list.txt
[source-ublock]: https://raw.githubusercontent.com/uBlockOrigin/uAssets/master/filters/filters.txt
[mirror-ublock]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/ublock/list.txt
[source-ublock-badware]: https://raw.githubusercontent.com/uBlockOrigin/uAssets/master/filters/badware.txt
[mirror-ublock-badware]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/ublock-badware/list.txt
[source-ublock-privacy]: https://raw.githubusercontent.com/uBlockOrigin/uAssets/master/filters/privacy.txt
[mirror-ublock-privacy]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/ublock-privacy/list.txt
[source-winhelp2002.mvps.org]: http://winhelp2002.mvps.org/hosts.txt
[mirror-winhelp2002.mvps.org]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/winhelp2002.mvps.org/list.txt
[source-zerodot1-coinblockerlists-browser]: https://zerodot1.gitlab.io/CoinBlockerLists/hosts_browser
[mirror-zerodot1-coinblockerlists-browser]: https://raw.githubusercontent.com/hectorm/hmirror/master/data/zerodot1-coinblockerlists-browser/list.txt

## Disclaimer

This script, by default, replaces the `/etc/hosts` file of your system. I am not responsible for any damage or loss, always make backups.

## License

See the [license](LICENSE.md) file.
