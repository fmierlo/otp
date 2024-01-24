# otp

`otp` is a single function implementation of [TOTP](https://tools.ietf.org/html/rfc6238)
in `bash` and a full rewrite of [matthauck/bash-totp](https://github.com/matthauck/bash-totp).

Since this implementation is a single function, it can be useful to be copied and
pasted into another script.

## Dependencies

- `openssl`
- `base32` with `coreutils` or `python3`
- `date` (`bash` < 5.0 only)

## Usage

Secret is a `base32` encoded string found in the field `secret` of a
[TOTP Key Uri Format](https://github.com/google/google-authenticator/wiki/Key-Uri-Format),
usually 16 chars long. Example:

    echo "hi world." | base32
    NBUSA53POJWGILQK

Or a full _TOTP Uri_, example:

    otpauth://totp/example.com:username%40example.com?secret=NBUSA53POJWGILQK&issuer=example.com

A secret must always be a file because `ps -e` can show environment variables.
Only the first line of the file will be parsed.

### With environment variable

Set the secret using a environment variable:

    OTP_SECRET_FILE=.secret ./otp

### Using stdin

Set the secret using the `cat` command:

    cat .secret | ./otp

Or the [pass](http://www.passwordstore.org/) command:

    pass otp/secret | ./otp

### Assigning other parameters

The digits, period and algorithm can be set too:

    OTP_DIGITS=6 OTP_PERIOD=30 OTP_ALGORITHM=SHA1 OTP_SECRET_FILE=otp.secret ./otp

### base32 implementation

The script auto-detect the available implementation, but it's possible to set
manually.

Standard linux or macOS with coreutils:

    OTP_BASE32=base32 OTP_SECRET_FILE=.secret ./otp

macOS without coreutils:

    OTP_BASE32=base32_py OTP_SECRET_FILE=.secret ./otp

### Inporting inside another script

    source ./otp

    OTP_SECRET_FILE=".secret"
    otp
