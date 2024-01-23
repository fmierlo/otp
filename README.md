# otp

`otp` is a single function implementation of [TOTP](https://tools.ietf.org/html/rfc6238)
in `bash` and a full rewrite of [matthauck/bash-totp](https://github.com/matthauck/bash-totp).

Since this implementation is a single function, it can be useful to be copied and
pasted into another script.

## Dependencies

- `openssl`
- `base32`
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

In case there are multiple valid secret lines in a file, only the first
occurrence of a valid secret will be used.

### With Environment Variable

Set the secret using a environment variable:

    OTP_SECRET_FILE=.secret ./otp

### Using Stdin

Set the secret using the `cat` command:

    cat .secret | ./otp

Or the [pass](http://www.passwordstore.org/) command:

    pass otp/secret | ./otp

### Assigning Other Parameters

The digits, period and algorithm can be set too:

    OTP_DIGITS=6 OTP_PERIOD=30 OTP_ALGORITHM=SHA1 OTP_SECRET_FILE=otp.secret ./otp

### Inporting Inside Another Script

    source ./otp

    OTP_SECRET_FILE=".secret"
    otp
