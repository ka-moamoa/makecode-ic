# MakeCode-Iceberg

MakeCode-Iceberg is a modification to [Microsoft's MakeCode](https://makecode.com) that automatically produces code that works despite intermittent, whole system power failures. MakeCode-Iceberg is currently configured to work with the Microbit version of MakeCode. This system uses compiler modifications to analyze and transform a user's code to interact with a custom FRAM driver for an external FRAM breakout board. 


## Project Overview


## How it Works

MakeCode-Iceberg is largely a modification to pxtcompiler/emitter/emitter.ts in the compileBinary() function. This system takes the Intermediate Representation(IR) output from the MakeCode pxt compiler and modifies it to work under harvested energy. Currently, the system performs a function inlining pass on the user's program to make the program control flow simpler. Next, the compiler analyzes the state of the program(global number variables) and builds FRAM read and write instructions for each one. Lastly, the compiler inserts checkpoints at the end of basic blocks that contain state modifing code(ex. an assignment to a global variable of interest) and inserts a restore operation at the top of the program to load checkpoints. The program will perform normal execution and save checkpoints until power is lost. The restore operation at the top of the program will check if a checkpoint is avaiable and load it if so, then jump back to the location of the last checkpoint where normal operation will resume.


## Getting Started

### Using our Server(Microbit)

1. Go to our [MakeCode-Iceberg Server](https://microbit-ic-jyd3j.ondigitalocean.app/)
2. Click "import" and then "import url"
3. Paste this github link https://github.com/chrispkraemer/fram-driver-iceberg
4. Start a new project
5. Add the fram driver as an extension by clicking on "Advanced" and then "Extensions"
6. Click on the FRAM Driver local extension to add it to your MakeCode project
7. Add `fram.init()` to the top of your program
8. Begin coding


### Building Locally

1. Install Node.js 8.9.4 or higher
2. Clone this repo

```
git clone https://github.com/ka-moamoa/makecode-ic
cd makecode-ic
```
3. Install dependencies
```
npm install
npm run build
cd ..
```
4. Clone pxt-common packages
```
git clone https://github.com/microsoft/pxt-common-packages
cd pxt-common-packages
npm install
cd ..
```
5. Clone the pxt-microbit repository
```
git clone https://github.com/microsoft/pxt-microbit
cd pxt-microbit
```
6. Install PXT command line
```
npm install -g pxt
```
7. Install pxt-microbit dependencies
```
npm install
```
8. Link each pxt directory from the microbit directory
```
pxt link ../pxt
pxt link ../pxt-common-packages
```
9. Run the server
```
pxt serve
```
10. Click "import" and then "import url"
11. Paste this github link https://github.com/chrispkraemer/fram-driver-iceberg
12. Start a new project
13. Add the fram driver as an extension by clicking on "Advanced" and then "Extensions"
14. Click on the FRAM Driver local extension to add it to your MakeCode project
15. Add `fram.init()` to the top of your program
16. Begin coding

# Microsoft MakeCode

* [Try out the editors in your browser...](https://makecode.com)

Microsoft MakeCode is based on the open source project [Microsoft Programming Experience Toolkit (PXT)](https://github.com/microsoft/pxt). ``Microsoft MakeCode`` is the name in the user-facing editors, ``PXT`` is used in all the GitHub sources.

PXT is a framework for creating special-purpose programming experiences for
beginners, especially focused on computer science education. PXT's underlying
programming language is a subset of TypeScript (leaving out JavaScript dynamic
features).

The main features of PXT are:
* a Blockly-based code editor along with converter to the text format
* a Monaco code editor that powers [VS Code](https://github.com/microsoft/vscode), editor's features are listed [here](https://code.visualstudio.com/docs/editor/editingevolved).
* extensibility support to define new blocks in TypeScript
* an ARM Thumb machine code emitter
* a command-line package manager

More info:
* [About](https://makecode.com/about)
* [Documentation](https://makecode.com/docs)

Examples of Editors built with MakeCode:

* https://makecode.microbit.org
* https://arcade.makecode.com
* https://makecode.adafruit.com
* https://minecraft.makecode.com
* https://makecode.mindstorms.com
* https://makecode.chibitronics.com
* More editors at https://makecode.com/labs

## Branches

* ``master`` is the active development branch, currently ``v3.*`` builds
* ``v*`` is the servicing branch for ``v*.*`` builds

## Running a target from localhost

Please follow the [instructions here](https://makecode.com/cli).

## Linking a target to PXT

If you are modifying your own instance of PXT and want a target (such as pxt-microbit) to use your local version, cd to the directory of the target (pxt-microbit, in our example, which should be a directory sibling of pxt) and perform

```
pxt link ../pxt
```

If you have multiple checkouts of pxt, you can do the following:
* run `npm i` in pxt and the target
* in the target, run `pxt link ..\some-other-pxt` (you may need to update your CLI first by running `npm install -g pxt`)

If you run `npm i` afterwards (in either the target or pxt), you might need to repeat these steps.

## Build

First, install [Node](https://nodejs.org/en/): minimum version 8.

To build the PXT command line tools:

```
npm install
npm run build
```

Then install the `pxt` command line tool (only need to do it once):

```
npm install -g pxt
```

After this you can run `pxt` from anywhere within the build tree.

To start the local web server, run `pxt serve` from within the root
of an app target (e.g. pxt-microbit). PXT will open the editor in your default web browser.

If you are developing against pxt, you can run `gulp watch` from within the root of the
pxt repository to watch for changes and rebuild.

```
gulp watch
```

If you are working on the CLI exclusively,

```
gulp watchCli
```

### Icons

There are a number of custom icons (to use in addition
to http://semantic-ui.com/elements/icon.html) in the `svgicons/` directory.
These need to be `1000x1000px`. Best start with an existing one. To see available icons go to
http://localhost:3232/icons.html (this file, along with `icons.css` containing
the generated WOFF icon font, is created during build).

If you're having trouble with display of the icon you created, try:
```
npm install -g svgo
svgo svgicons/myicon.svg
```

### Documentation Highlighting

In the documentation, highlighting of code snippets uses highlight.js (hljs).
Currently, the following languages are included:

* TypeScript
* Python
* JavaScript
* HTML,XML
* Markdown

If you need to add other languages or update existing ones,
you can find the distribution at [https://highlightjs.org/download/](https://highlightjs.org/download/);
select all the languages you want to include (including the ones above!),
download and unzip,
and finally copy over `highlight.pack.js` into `webapp/public/highlight.js/`.

## Tests

The tests are located in the `tests/` subdirectory and are a combination of node and
browser tests. To execute them, run `npm run test:all` in the root directory.

## License

[MIT License](https://github.com/microsoft/pxt/blob/master/LICENSE)

## Code of Conduct

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Contact Us

[Get in touch](https://makecode.com/contact)

## Trademarks

MICROSOFT, the Microsoft Logo, and MAKECODE are registered trademarks of Microsoft Corporation. They can only be used for the purposes described in and in accordance with Microsoft’s Trademark and Brand guidelines published at https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general.aspx. If the use is not covered in Microsoft’s published guidelines or you are not sure, please consult your legal counsel or MakeCode team (makecode@microsoft.com).
