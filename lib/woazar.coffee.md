# Woazar

> Generate UUID/serials/hashes easily from the command line.

[https://github.com/Leny/woazar](https://github.com/Leny/woazar)
Copyright (c) 2014 Leny
Licensed under the MIT license.

* * *

## lib/woazar.js

> Main entry point

* * *

    "use strict"

We use the [commander](https://npmjs.org/package/commander) & [chalk](https://npmjs.org/package/chalk) packages.

    woazar = require "commander"
    chalk = require "chalk"
    cliparoo = require "cliparoo"

Load `package.json` and *woazar* modules.

    pkg = require "../package.json"

    modules =
        hash: require "./modules/hash.js"
        uuid: require "./modules/uuid.js"
        timestamp: require "./modules/timestamp.js"
        replace: require "./modules/replace.js"

Define program commands (using commander's API).

    woazar
        .version( pkg.version )
        .usage( "[options] <sentence>" )
        .option( modules.hash.options, modules.hash.description )
        .option( modules.uuid.options, modules.uuid.description )
        .option( modules.timestamp.options, modules.timestamp.description )
        .option( modules.replace.options, modules.replace.description )
        .option( "-c, --copy", "Copy the result to the clipboard." )

    woazar.parse process.argv

Redirect the arguments to specified modules.

    if !!woazar.uuid
        result = modules.uuid.exec()
    else if !!woazar.timestamp
        result = modules.timestamp.exec woazar.args.join( " " ).trim()
    else
        if not ( sentence = woazar.args.join( " " ).trim() )
            console.log chalk.bold.red( "ERROR: " ), "no sentence given !!!"

        if !!woazar.hash
            result = modules.hash.exec woazar.hash, sentence
        else if !!woazar.replace
            result = modules.replace.exec sentence

    if !!woazar.copy
        cliparoo result
        result += chalk.italic.green " (copied to the clipboard)"

    console.log result

> TODO : follow the specs : http://files.flatland.be/fFHu
