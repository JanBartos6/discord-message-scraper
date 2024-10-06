Firstly, insert your user token to user_token.txt.
Then, you can go to public_servers.ipynb and execute it (except the last block which is unfinished).
In public_servers.ipynb, you'll find code for bypassing CloudFlare and then extracting discord invites from Disboard.com based on topic from topics.txt
Servers are by default sorted by member-count and you can estimate the number of invites from number of scrolls (scraping all servers bigger than X members feature commming in the next update).

The next step is to get though the redirect from disboard invites to discord invites (also featuring the Clouflare bypass).

Accepting invites and server verification currently in developement.

Experimental message scraping

First, cd to C:\path\to\DiscordChatExporter in your terminal (message_scraper/version_based_on_your_system)

To execute with the default settings, type: ./DiscordChatExporter.Cli


| Command                 | Description                                          |
|-------------------------|------------------------------------------------------|
| export                  | Exports a channel                                    |
| exportdm                | Exports all direct message channels                  |
| exportguild             | Exports all channels within the specified server     |
| exportall               | Exports all accessible channels                      |
| channels                | Outputs the list of channels in the given server     |
| dm                      | Outputs the list of direct message channels          |
| guilds                  | Outputs the list of accessible servers               |
| guide                   | Explains how to obtain token, server, and channel ID |

How to get your user token: https://www.youtube.com/watch?v=jjtu0VQXV7I

Customization

Export a specific channel

You can quickly export with DCE's default settings by using just -t token and -c channelid. (-f format - json, csv, html,...). You can also change the export directory by using -o.

./DiscordChatExporter.Cli export -t "mfa.Ifrn" -c 53555 -f json -o "C:\Discord Exports"

If any of the folders in the path have a space in its name, escape them with quotes (").

#### Generating the filename and output directory dynamically

You can use template tokens to generate the output file path based on the server and channel metadata.

```console
./DiscordChatExporter.Cli export -t "mfa.Ifrn" -c 53555 -o "C:\Discord Exports\%G\%T\%C.html"
```

Assuming you are exporting a channel named `"my-channel"` in the `"Text channels"` category from a server
called `"My server"`, you will get the following output file
path: `C:\Discord Exports\My server\Text channels\my-channel.html`

Here is the full list of supported template tokens:

- `%g` - server ID
- `%G` - server name
- `%t` - category ID
- `%T` - category name
- `%c` - channel ID
- `%C` - channel name
- `%p` - channel position
- `%P` - category position
- `%a` - the "after" date
- `%b` - the "before" date
- `%%` - escapes `%`

Partitioning

You can use partitioning to split files after a given number of messages or file size. For example, a channel with 36 messages set to be partitioned every 10 messages will output 4 files.

./DiscordChatExporter.Cli export -t "mfa.Ifrn" -c 53555 -p 10

A 45 MB channel set to be partitioned every 20 MB will output 3 files.

./DiscordChatExporter.Cli export -t "mfa.Ifrn" -c 53555 -p 20mb

Date ranges

Messages sent before a date Use --before to export messages sent before the provided date. E.g. messages sent before September 18th, 2019:

./DiscordChatExporter.Cli export -t "mfa.Ifrn" -c 53555 --before 2019-09-18

Messages sent after a date Use --after to export messages sent after the provided date. E.g. messages sent after September 17th, 2019 11:34 PM:

./DiscordChatExporter.Cli export -t "mfa.Ifrn" -c 53555 --after "2019-09-17 23:34"

Including threads

By default, threads are not included in the export. You can change this behavior by using --include-threads and specifying which threads should be included. It has possible values of none, active, or all, indicating which threads should be included. To include both active and archived threads, use --include-threads all.

./DiscordChatExporter.Cli exportguild -t "mfa.Ifrn" -g 21814 --include-threads all

Export all channels

To export all accessible channels, use the exportall command:

./DiscordChatExporter.Cli exportall -t "mfa.Ifrn"

Excluding DMs

To exclude DMs, add the --include-dm false option.

./DiscordChatExporter.Cli exportall -t "mfa.Ifrn" --include-dm false

List channels in a server

To list the channels available in a specific server, use the channels command and provide the server ID through the -g|--guild option:

./DiscordChatExporter.Cli channels -t "mfa.Ifrn" -g 21814

List servers

To list all servers accessible by the current account, use the guilds command:

./DiscordChatExporter.Cli guilds -t "mfa.Ifrn" > C:\path\to\output.txt

