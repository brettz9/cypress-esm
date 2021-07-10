# cypress-esm

Steps to reproduce:

1. Run `npm install`
2. Run `npm test` (which opens `cypress open`)
3. Observe the error message that appears.

The components of significance are:

1. `type: "module"` in `package.json` (which sets the default for `.js` files
    in Node.js to be treated as ESM modules). If removed, the steps above will
    not fail.
2. Even if changing the contents of `/cypress/plugins/index.js`, the plugins
    file does not work with ESM-style `export default {}`. Working around it
    to change the plugins file to the `.cjs` extension (which is always
    interpreted by Node.js as a CommonJS file), i.e., to
    `/cypress/plugins/index.cjs` (and using `module.exports`) similarly
    does not work.
