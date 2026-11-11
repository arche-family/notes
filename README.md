# Notes

The Arche phone's notes, written in [Arche](https://github.com/arche-family/arche-lang): a Nimadir mini-app, a WebAssembly component of Nimadir's world, which the phone has from its first start and Nimadir runs on a desktop.

It works like Android's notes apps:

- The notes are cards, the one changed last first. Each shows its title and the start of its text, its lines run together and cut with … when it is long.
- New note opens an empty note, its title field given the keyboard.
- A card opens its note: its title and its text, a field each, the text one of many lines. What is typed is kept as it is typed, and the note goes to the top of the list.
- Back returns to the list; a note left with nothing in it is not kept. Delete takes the note away.

The notes are kept in the app's storage, a line each: the note's number, title and text, parted by tabs, with `\` escaping a tab (`\t`), a line end (`\n`) and itself. It is an app of the page: its taps come as click events, and its typing as input events.

## Build

With [arche-lang](https://github.com/arche-family/arche-lang) checked out next to this repository and built (`make -C stage0 && make -C compiler && make -C wasm`):

```sh
../arche-lang/wasm/nimadir-app.sh . notes.component.wasm
```

`manifest` is what the phone's repository packages with the component. It gives the app's name, version and title, and what it may do: keep its data (`storage`), the notes.

## Test

`headless` holds the taps and the typing of a headless run (a note made, typed into and kept; an empty one not kept; the first opened, changed and deleted; a long one, its card's text cut short), and `expected.txt` holds what Nimadir's own shell said to them. `app-check.sh` runs the app with arche-wasm, by the interpreter and by the JIT, and compares what it says with `expected.txt`. CI runs it on every push.

```sh
../arche-lang/wasm/app-check.sh .
NIMADIR=~/nimadir ../arche-lang/wasm/app-expected.sh .   # expected.txt again, from Nimadir's shell
```

## License

MIT or Apache-2.0, at your option ([LICENSE](LICENSE)).
