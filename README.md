# midi2abcWASM

A WebAssembly port of [abcmidi](https://github.com/sshlien/abcmidi)'s `midi2abc`, allowing MIDI-to-ABC conversion in the browser.

[Check the Demo here](https://reinissance.github.io/midi2abcWASM/)

## Features

- Upload MIDI files and convert to ABC notation in-browser
- Renders ABC notation using [abcjs](https://abcjs.net/)

## Usage

1. Clone this repo (with submodules):

   ```sh
   git clone --recurse <repo-url>
   ```

2. Build the WASM module (see below).

3. Open `index.html` in your browser.

## Building

You need [Emscripten](https://emscripten.org/) to build the WASM version of midi2abc or yaps.

First, install and activate the latest emsdk (Emscripten SDK):

```sh
git submodule update --init --recursive
cd emsdk
./emsdk install latest
./emsdk activate latest
source ./emsdk_env.sh
cd ..
```

To build midi2abc:
```sh
# Example build command (adjust as needed)
emcc abcmidi/midi2abc.c abcmidi/midifile.c -o midi2abc.js -s WASM=1 -O2 \
  -s "EXPORTED_FUNCTIONS=['_main']" -s "EXPORTED_RUNTIME_METHODS=['FS','callMain']" \
  -s MODULARIZE=1 -s EXPORT_NAME='midi2abcModule'
```
You could also build yaps to render (though I was happier using abcjs):
```sh
# Example build command (adjust as needed)
emcc abcmidi/yapstree.c abcmidi/parseabc.c abcmidi/drawtune.c abcmidi/debug.c abcmidi/pslib.c abcmidi/position.c abcmidi/parser2.c abcmidi/music_utils.c \
  -o yaps.js -s WASM=1 -O2 \
  -s "EXPORTED_FUNCTIONS=['_main']" -s "EXPORTED_RUNTIME_METHODS=['FS','callMain']" \
  -s MODULARIZE=1 -s EXPORT_NAME='yapsModule' \
  -s INITIAL_MEMORY=536870912 -s MAXIMUM_MEMORY=2147483648 -s ALLOW_MEMORY_GROWTH=1
```

## License

This project is based on [abcmidi](https://github.com/sshlien/abcmidi), which was licensed under the GNU GPL v2 or later. See [LICENSE](LICENSE) for details.

## Credits

- [abcmidi](https://github.com/sshlien/abcmidi)
- [abcjs](https://abcjs.net/)
- [emsdk (Emscripten SDK)](https://emscripten.org/)

