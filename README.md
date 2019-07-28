# JavaScript Development Program Tutorials

## Installation

First you need to install `nodejs`. To install it you follow the official [documentation](https://nodejs.org/en/download/) or you can use [nvm](https://github.com/nvm-sh/nvm).

After installing `nodejs` you need to install `http-server` using `npm`. `npm` already comes with `nodejs`.

To do that run the following command:

```sh
npm install -g http-server
```

## Run Examples

Locate under the example you wanna run and then execute `http-server`.

```sh
cd examples/mostly-fluid-layout
http-server -p 8080 -a localhost -c
```

This will start a server at `localhost:8080` that's gonna serve the content under `mostly-fluid-layout`. Open your preferred browser and go to `localhost:8080` to see the example.
