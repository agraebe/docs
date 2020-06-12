# Welcome to the Blockstack documentation repo!

This README explains the user cases, source file organization, and procedures for building the Blockstack documentation.  You can find the documentation at https://docs.blockstack.com

You can also make use of the **Edit this page on GitHub** link from any https://docs.blockstack.org page.

## Authoring Environment Setup

When setting up your machine for the first time, run:

```bash
#install Ruby Version Manager (rvm.io/rvm/install)
\curl -sSL https://get.rvm.io | bash -s stable
# restart terminal or run the following source command:
source ~/.rvm/scripts/rvm
# install Ruby 2.7 and the needed dependencies
rvm install 2.6
rvm use 2.6 --default
gem install bundler
# make sure you're in the root of your clone of this repo and run
bundle install
```

Then when authoring, run:

```bash
jekyll serve
```

You can preview your changes by visiting `http://localhost:4000`

## Docker Instructions for Authoring

Don't want to install a bunch of stuff and don't mind the overhead of using Docker? Do this instead:

```bash
docker build -t blockdocs .
```

From then on, you can run:

```bash
docker run -p 4000:4000 -ti -v "$(pwd)":/usr/src/app blockdocs
```

## Documentation backend

Our documentation is written in Markdown (`.md`), built using [Jekyll](https://jekyllrb.com/), and deployed to a Netlify server. Serving the content from Netlify allows us to use functionality (plugins/javascript) not supported with standard GitHub pages.

Some content is single sourced. Modifying single source content requires an understanding of that concept, how it works with Liquid tags, and the organization of this repo's source files.

[UIKit](https://getuikit.com/docs/introduction) provides the documentation theme. There is explicit use of HTML plus UIKit components directly in files where needed for special layouts. And there is use of CSV and JSON source files transformed with [Liquid template language](https://help.shopify.com/en/themes/liquid) to produce reference content.

## How the Source Files are Organized

Directories that contain information used to build content.

<table>
  <tr>
    <th>Directory</th>
    <th>Purpose</th>
     <th>Technical Repo(s)</th>
  </tr>
  <tr>
    <td>android</td>
    <td>SDK tutorial.</td>
   <td><a href="https://github.com/blockstack/blockstack-android">https://github.com/blockstack/blockstack-android</a></td>
  </tr>
  <tr>
    <td>browser</td>
    <td>Information for end-users about our identity, Storage, and using the browser.</td>
   <td><a href="https://github.com/blockstack/blockstack-browser">https://github.com/blockstack/blockstack-browser</a></td>
  </tr>
  <tr>
    <td>common</td>
    <td>Contains several shell files that redirect to our reference documentation sites such as Javascript, IOS, and so forth.  The reference docs are linked from the developer, core, and Gaia menus.</td>
    <td>Each of these references are generated by their respective repos, core.blockstack.org from <code>blockstack-core</code>, Javascript docs from the <code>blockstack.js</code> and so forth.</td>
  </tr>
  <tr>
    <td>core</td>
    <td>Information for wallet, blockchain, or Clarity developers -- including Atlas, BNS, and so forth. <b>This content STILL needs to be synced with the master docs subdirectory in <a href="https://github.com/blockstack/blockstack-core/tree/master/docs">blockstack-core</a>.</b></td>
    <td> <a href="https://github.com/blockstack/blockstack-core/tree/master/docs">blockstack-core</a></td>
  </tr>
  <tr>
    <td>develop</td>
    <td>Information for application developers covers using the Javascript library, our mobile SDKs, and the concepts hat support them.</td>
    <td>This information was never associated with a single repo. Much of it does rely heavily on <a href="https://github.com/blockstack/blockstack.js">https://github.com/blockstack/blockstack.js</a>.</td>
  </tr>
    <tr>
    <td>faqs</td>
    <td>Contains files for single-sourcing all the FAQs. The Blockstack docs has a single page that <a href="https://docs.blockstack.org/faqs/allfaqs">lists all the faqs</a>; then several pages in different sections re-use this information. See the FAQs section below for detail about how these files figure into FAQS.</td>
    <td>Not related to repo.</td>
  </tr>
  <tr>
  <td>-includes</td>
    <td>Information reused (markdown or html) in many places, common html used in pages and notes. </td>
    <td>These files don't correspond to a repository.</td>
  </tr>
    <tr>
    <td>ios</td>
    <td>SDK tutorial.</td>
    <td><a href="https://github.com/blockstack/blockstack-ios">https://github.com/blockstack/blockstack-ios</a></td>
  </tr>
  <tr>
    <td>org</td>
    <td>Information for Stacks holders and people curious about what Blockstack does</td>
    <td>Not associated with any repository.</td>
  <tr>
    <td>storage</td>
    <td>Information for developers using storage in their apps or creating Gaia servers.</td>
       <td><a href="https://github.com/blockstack/blockstack-gaia">https://github.com/blockstack/blockstack-gaia</a></td>
  </tr>
  </table>

These are the other directories in the site structure:

<table>
  <tr>
    <th>Directory</th>
    <th>Purpose</th>
  </tr>
  <tr>
    <th>-data</th>
    <td>JSON source files for the FAQS, CLI, and clarity reference. Menu configurations YAML files. The CSV file with the glossary. </td>
  </tr>
<tr>
    <th>-layouts</th>
    <td>Layouts for various pages. The community layout is significantly different from the other layouts.</td>
</tr>
  <tr>
    <th>-sass</th>
    <td>Style folder including customizations.</td>
  </tr>
 <tr>
    <th>assets</th>
    <td>Support for the docs templates.</td>
  </tr>
  </table>

## Deployment of the site

The documentation is deployed to Netlify using the following command:

```
JEKYLL_ENV=production bundle exec jekyll build --config _config.yml
```

## Generated documentation

### To generate the CLI json manually

The `_data/cliRef.json` file is generated from the `blockstack-cli` subcommand `docs`. This data file is consumed by the `_includes/commandline.md` file which is used to serve up the reference.  

1. Install the latest version of the cli according to the instructions at: https://github.com/blockstack/cli-blockstack

2. Generate the json for the cli in the `docs.blockstack` repo.

   ```
   $ blockstack-cli docs | python -m json.tool > _data/cliRef.json
   ```

3. Make sure the generated docs are clean by building the documentation.

   If you run into any problem in the generation usually it results from a problem in the repo. You can make a pull request back to the repo to fix anything.

### Clarity Reference

As of 8/12/19 Clarity is in the [develop](https://github.com/blockstack/blockstack-core/tree/develop) branch of core.  You can build the Clarity command line from the Docker image. `core/src/vm/docs/mod.rs`


1. Pull the latest developer preview from the Docker Hub.

   ```
   $ docker pull blockstack/blockstack-core:clarity-developer-preview
   ```

2. Build the lastest JSON.

   ```
   docker run -it blockstack/blockstack-core:clarity-developer-preview blockstack-core docgen | jsonpp > ~/repos/docs.blockstack/_data/clarityRef.json
   ```

3. Build the documentation and verify the Clarity reference is building correctly.

4. Make changes in core
5. Build the docker image
6. Run doc gen with the new image

    ```
   $ docker run --name docsbuild -it blockstack-test blockstack-core docgen | jsonpp > ~/repos/docs.blockstack/_data/clarityRef.json
    ```

    This generates the JSON source files which are consumed through Liquid templates into markdown.

7. Rebuild the documentation site with Jekyll.

8. Review the changes in the Clarity documentation to ensure that no breaking changes were introduced through code changes.

9. If you find the documentation is no longer formatting correctly or there are errors in the reference, create a PR against the `blockstack-core` repo.

### View and test the clarity cli

You can view [the source code](https://github.com/blockstack/blockstack-core/blob/develop/src/clarity.rs).

1. Pull the Stacks Node clarity-developer-preview image from Docker Hub.

   ```bash
    $ docker pull blockstack/blockstack-core:clarity-developer-preview
   ```

2. Start the Stacks Node test environment with a Bash shell.

    ```bash
    $ docker run -it -v $HOME/blockstack-dev-data:/data/ blockstack/blockstack-core:clarity-developer-preview bash
    ```

    The command launches a container with the Clarity test environment and opens a bash shell into the container.

3. Run the clarity-cli in the shell.

    ```bash
    root@5b9798633251:/src/blockstack-core# clarity-cli
    Usage: clarity-cli [command]
    where command is one of:

    initialize         to initialize a local VM state database.
    mine_block         to simulated mining a new block.
    get_block_height   to print the simulated block height.
    check              to typecheck a potential contract definition.
    launch             to launch a initialize a new contract in the local state database.
    eval               to evaluate (in read-only mode) a program in a given contract context.
    eval_raw           to typecheck and evaluate an expression without a contract or database context.
    repl               to typecheck and evaluate expressions in a stdin/stdout loop.
    execute            to execute a public function of a defined contract.
    generate_address   to generate a random Stacks public address for testing purposes.
    ```

## Understand how the shared FAQs work

The FAQ system servers single-sourced content that support the FAQs that appear in blockstack.org, and stackstoken.com site. We have FAQs that fall into these categories:

* general
* appusers
* dappdevs
* coredevs
* opensource
* miscquest
* wallet  (Wallet users)
* tokens (Stacks owners)

FAQs are usually written internally by a team that are unfamiliar with markdown or HTML used in websites. So, FAQs are produced using this process:

1. Draft new or revised FAQs in a Google or Paper document.
2. Review the drafts and approve them.
3. Convert the FAQ document to HTML.
4. Strip out the unnecessary codes such as `id` or `class` designations.
   This leaves behind plain html.
5. Add the new FAQs to the `_data/theFAQS.json` file.
   Currently this is manually done through cut and paste.
6. Copy the JSON for `appminers` categories to the `_data/appFAQ.json` file.
7. Run the Jekyll build and verify the content builds correctly by viewing this `LOCAL_HOST/faqs/allfaqs`
8. Push your changes to the site and redeploy.

 Single sourcing the content ensures that FAQs are the same regardless of where and on which property they appear in. These source files play a role in the FAQ single-sourcing layout:

<table>
  <tr>
    <th>File </th>
    <th>Contains</th>
    <th>Purpose</th>
  </tr>
  <tr>
    <td>_faqs/allfaqs.json</td>
    <td>Liquid to generate JSON from theFAQS.json</td>
    <td>Serves up the FAQs that are consumed by the blockstack.org and stackstoken.com sites.</td>
  </tr>
  <tr>
    <td>_faqs/allFAQs.md</td>
    <td>Liquid and headings.</td>
    <td>Reads the _data/theFAQs.json and produces a Markdown file for display in our site.</td>
  </tr>
  <tr>
    <td>_data/theFAQs.json</td>
    <td>JSON for the FAQS.</td>
    <td>Source file for all the FAQs.</td>
  </tr>
</table>
