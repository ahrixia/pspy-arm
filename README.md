# pspy-arm

Prebuilt **ARM64 / ARM32** binaries of [`pspy`](https://github.com/DominicBreuker/pspy) for the **Raspberry Pi** and other ARM Linux boards.

> 🙏 **All credit to the original author [Dominic Breuker](https://github.com/DominicBreuker) and the upstream project [DominicBreuker/pspy](https://github.com/DominicBreuker/pspy).**
> This repo contains **no code changes** — just the upstream source cross-compiled for ARM, which upstream doesn't ship prebuilt. I made it so I could run pspy on my Raspberry Pi without installing the whole Go toolchain on it.

✅ **Tested on a Raspberry Pi 4.**

---

## Which file do I need?

```bash
uname -m
```

| `uname -m` | Download | Boards |
|---|---|---|
| `aarch64` / `arm64` | **`pspy-arm64`** | Pi 3 / 4 / 5, Zero 2 W (64-bit OS) |
| `armv7l` | **`pspy-arm32v7`** | Pi 2 / 3 / 4 (32-bit OS) |
| `armv6l` | **`pspy-arm32v6`** | Pi 1 / Zero / Zero W |

Grab the matching binary from the [**Releases**](../../releases) page.

---

## Run it

```bash
chmod +x pspy-arm64
./pspy-arm64 -pf
```

- `-p` print new commands/processes
- `-f` print file events
- `-i 1000` scan interval in ms (lower catches shorter-lived commands)

Single static binary, nothing gets installed — delete it with `rm` when done. Run it from `/dev/shm` if you want to leave the disk untouched. Use `sudo` to see other users' full command lines. Full docs upstream: <https://github.com/DominicBreuker/pspy#readme>

---

## Build your own (newer upstream versions)

I don't always have time to push updated builds, but it's a 2-liner. On any machine with Go (`sudo apt install golang-go`), cross-compile the upstream source for your Pi:

```bash
git clone https://github.com/DominicBreuker/pspy && cd pspy

# 64-bit Pi (aarch64):
CGO_ENABLED=0 GOOS=linux GOARCH=arm64 go build -ldflags "-s -w" -o pspy-arm64

# 32-bit Pi (armv7l):
CGO_ENABLED=0 GOOS=linux GOARCH=arm GOARM=7 go build -ldflags "-s -w" -o pspy-arm32v7
```

Then `scp` the binary to your Pi. That's exactly how the files here were built.

---

## License & disclaimer

`pspy` is licensed **Apache-2.0** by its original author; this repo redistributes unmodified builds under that same license (see `LICENSE`). Provided as-is, no warranty. Use only on systems you own or are authorized to monitor.
