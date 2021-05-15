# GooseUpdate

A HTTP server which replicates Discord's update API, proxying requests to the official endpoint and allows custom versions of modules.

# Deploying
1. Install GooseUpdate's dependencies with `npm install`
2. Copy `example.config.js` to `config.js` and modify it to your liking, then run `node src/index.js (PORT)`.

# Usage
Discord fetches the update API URL from a `settings.json` file stored in various directories depending on your operating system.

Said directories are found below:
* Windows:
  * `%appdata%\discord<channel>\`
* Mac:
  * `~/Library/Application Support/discord<channel>/`
* Linux:
  * `~/.config/discord<channel>/`

Set `UPDATE_ENDPOINT` and `NEW_UPDATE_ENDPOINT` in `settings.json` as follows:

```json
"UPDATE_ENDPOINT": "https://<GooseUpdate instance URL>/branch"
"NEW_UPDATE_ENDPOINT": "https://<GooseUpdate instance URL>/branch/"
```

GooseUpdate also supports including multiple branches in updates by separating their names with a `+`, like `https://<GooseUpdate instance URL>/branch1+branch2`.

# Adding a branch
GooseUpdate branches patch `discord_desktop_core` with files stored in `branches/<branch name>/`.

Branches must have a `patch.js` file to handle their injection in their branch folder.

`patch.js` files have their version stored at the top of them in a META block, an example patch.js is below:

```javascript
/*META
{
  "version": 1
}*/
// Any code you want to inject goes here
require('mod.js')
```

`patch.js` files MUST have their version increased when they're changed or the Discord client will not download updates to them.

Other files can be included in your branch folder, but are not required.