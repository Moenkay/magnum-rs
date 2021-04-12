# Magnum (Opus Tools)

[![LICENSE](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

Provides support for decoding Xiph.org's Opus Audio codec from a Rust Reader.
The Opus audio can be in either the standard Ogg container format, or in
Apple Core Audio (.caf) format.

Features are provided that enable support for outputting as a Kira AudioStream,
as well as Rodio's Source.

## Features & Compatibility

By default this library provides Ogg and Caf container support, but if you
wish to only enable support for one, you can manually choose by providing
either `with_ogg` or `with_caf` in the feature list in your `Cargo.toml`

Enabling features for the following provides associated traits needed for
compatibility with those libraries, however you need to have a version of
those libraries that exactly matches with the one used in this library.
(See the Version column for the currently supported one)

| Feature      | Adds Support For                            | Version |
| ------------ | ------------------------------------------- | ------- |
| `with_kira`  | [Kira](https://github.com/tesselode/kira)   | 0.5.1   |
| `with_rodio` | [Rodio](https://github.com/RustAudio/rodio) | 0.13.1  |

## Example Usage

### Using Magnum with [Rodio](https://github.com/RustAudio/rodio):

Add to your `Cargo.toml`'s dependencies section:

```toml
[dependencies]
magnum = { version = "*", features = ["with_rodio"] }
```

In your application code:

```rust
// Dependencies
use rodio::{OutputStream, Sink};

// ...

// Set up your OutputStream as usual in Rodio
let (_stream, stream_handle) = OutputStream::try_default().unwrap();

// Use a BufReader to open an opus file in Ogg format (in this example)
let file = BufReader::new(File::open("example.opus").unwrap());

// Pass the reader into Magnum's OpusSourceOgg to get a Source compatible with Rodio
let source = magnum::container::ogg::OpusSourceOgg::new(file).unwrap();

// Create a Sink in Rodio to receive the Source
let sink = Sink::try_new(&stream_handle).unwrap();

// Append the source into the sink
sink.append(source);

// Wait until the song is done playing before shutting down (As the sound plays in a separate thread)
sink.sleep_until_end();
```

## TODOs

- [ ] Tests
- [ ] Better Error Handling
- [ ] Examples
- [ ] More Container Formats (.mkv, etc)
