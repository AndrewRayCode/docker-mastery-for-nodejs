# Debugging

See [typescript/nodemon.json](typescript/nodemon.json)

Enabling debugger in nodemon.json (has node command): note you have to use four
zeroes for IP, they by default will only run on localhost, but in the container,
localhost is just the container

This setup in the example is *not* specific to VSCode, although with vscode
tasks you can do cool stuff. This example is cross-editor compatible, using
nodemon to watch and recompile