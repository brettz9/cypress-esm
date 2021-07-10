# cypress-esm

Steps to reproduce:

1. Clone the repository
2. Run `npm install`
3. Run `npm test` (which opens `cypress open`)
4. Observe the error message that appears.

The components of significance are:

1. `type: "module"` in `package.json` (which sets the default for `.js` files
    in Node.js to be treated as ESM modules). If removed, the steps above will
    not fail, and one can use ESM in other files when there is no
    `type: "module"` by using the file extension `.mjs` on those files, but
    this is an undue burden on projects because many projects wish to keep
    `type: "module"` so that the default behavior of `.js` at present will be
    changed so that `.js` files will instead be interpreted as ESM files.
    Requiring that all other files in a project use `.mjs` is a burden on
    projects.
2. One would hope that one could change the contents of
    `/cypress/plugins/index.js` (which is now interpreted by Node as an ESM
    file because of `type: "module"`). However, Cypress does not allow the
    plugins file to work with ESM-style `export default {}` exports as one
    would hope. While in theory, Cypress, could require CJS for now and require
    that users change the plugins file to use the `.cjs` extension (which
    is always interpreted by Node.js as a CommonJS file), thus allowing users
    to continue to use `module.exports` in their plugins file until such time
    as Cypress may support ESM-style exports in plugins files, changing the
    file to `/cypress/plugins/index.cjs` unfortunately does not work at
    present either, even if setting a `pluginsFile` option which points to a
    `cjs` file because Cypress apparently does not support a plugins file with
    a `.cjs` extension.
