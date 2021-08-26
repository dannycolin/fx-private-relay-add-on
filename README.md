# Private Relay
Private Relay generates email aliases to use in place of personal email addresses.

Recipients will still receive emails, but Private Relay keeps their personal
email address from being [harvested](https://blog.hubspot.com/marketing/what-is-a-landing-page-ht), 
and then [bought, sold, traded, or combined](https://www.bookyourdata.com/) 
with  other data to personally identify, track, and/or [target
them](https://www.facebook.com/business/help/606443329504150?helpref=faq_content).

## Usage (for now)

1. Install the extension.

2. Go to [relay.firefox.com](https://relay.firefox.com) and sign in.

3. In any `<input>` element, right-click and select "Make a relay address"
   * The extension will populate the options with your relay addresses.


## Local Extension Development

1. Clone, change to the directory, install dependencies:

    ```
    git clone --recurse-submodules git@github.com:mozilla/fx-private-relay-add-on.git
    npm install
    ```

2. Run with
   [`web-ext`](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/Getting_started_with_web-ext):

    ```
    web-ext run -s src/
    ```
    
    or via npm:

    ```
    npm run dev
    ```

3. Visit http://127.0.0.1:8000


### Working with translations
#### Getting the latest translations
We use a [git submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules)
for translated message files. The `--recurse-submodules` step of installation
should bring the message files into your working directory already, but you may
want also want to udpate the translations after install. The easiest way to do
that is:

* `git submodule update --remote`

#### Add/update messages for translation
The `privaterelay/locales` directory is a git repository like any other, so to
make changes to the messages:

1. Make whatever changes you need in `src/_locales/en` as you work.

2. `cd src/_locales/en`

3. `git branch message-updates-yyyymmdd`

4. `git push -u origin message-updates-yyyymmdd`

You can then open a pull request from the `message-updates-yyyymmdd` branch to
[the l10n repo](https://github.com/mozilla-l10n/fx-private-relay-add-on-l10n/) `main` branch.

## Build for other environments

These scripts will build the add-on to work with dev, stage, or prod servers.

 * `npm run build:dev`: https://dev.fxprivaterelay.nonprod.cloudops.mozgcp.net/
 * `npm run build:stage`: https://stage.fxprivaterelay.nonprod.cloudops.mozgcp.net/
 * `npm run build:prod`: https://relay.firefox.com/

### Distributing
#### Make the new version

1. Bump the version number in `package.json` and `manifest.json`
2. Commit the version number bump
3. Create a git tag for the version: `git tag <version>`
4. Push the tag up to GitHub: `git push --tags`

#### Publish to AMO

1. `npm run-script build`
2. [Upload the `.zip` to AMO](https://addons.mozilla.org/en-US/developers/addon/private-relay/versions/submit/)

#### Publish to GitHub
Finally, we also publish the release to GitHub for those followers.

1. Download the signed `.xpi` from [the addon versions page](https://addons.mozilla.org/en-US/developers/addon/private-relay/versions)
2. [Make the new release on
   GitHub](https://github.com/mozilla/fx-private-relay-add-on/releases/new)
   * Use the version number for "Tag version" and "Release title"
   * Release notes: copy the output of `git log --no-merges --pretty=format:"%h %s" <previous-version>..<new-version>`
   * Attach binaries: select the signed `.xpi` file
