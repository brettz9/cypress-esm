# cypress-esm

Steps to reproduce:

1. Clone the repository
2. Run `npm install`
3. Run `npm test` (which opens `cypress open`)
4. Observe the error message that appears.

The components of significance are:

1. `type: "module"` in `package.json` (which sets the default for `.js` files
    in Node.js to be treated as ESM modules). If removed, the steps above will
    not fail. One can use ESM modules without `type: "module"` by using the
    file extension `.mjs`, but many projects wish to change the default
    behavior of `.js` so that it too can be interpreted as an ESM file. So
    requiring that all other files use `.mjs` is a burden on projects.
2. Even if changing the contents of `/cypress/plugins/index.js` (which is
    now interpreted as an ESM file because of `type: "module"`), the plugins
    file does not work with ESM-style `export default {}` as would be expected.
    While in theory one should be able to change the plugins file to use the
    `.cjs` extension (which is always interpreted by Node.js as a CommonJS
    file), thus allowing us to use `module.exports` until such time as
    Cypress may support ESM plugins files, i.e., by changing the file to
    `/cypress/plugins/index.cjs`, this unfortunately does not work either, even
    if setting a `pluginsFile` option which points to a `cjs` file.
