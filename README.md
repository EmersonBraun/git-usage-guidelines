# BEX git (git-everi)

BEX Git everi

## Install the dependencies
```bash
npm install
```

### Start the app in development mode (hot-code reloading, error reporting, etc.)

#### Install global quasar
```bash
npm install -g @quasar/cli
```

```bash
quasar dev -m bex
```

### Lint the files
```bash
npm run lint
```

### Build the app for production
```bash
quasar build -m bex
```
the generated files will be in: 
```bash
quasar build -m bex
```
dist
    |_bex
        |_Packaged
            |_chrome
            |_firefox
        |_Unpackaged
            |_www

### Customize the configuration
See [Configuring quasar.conf.js](https://quasar.dev/quasar-cli/quasar-conf-js).
