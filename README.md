# otp

`otp` is a single function implementation of [TOTP](https://tools.ietf.org/html/rfc6238)
in `bash` and a full rewrite of https://github.com/matthauck/bash-totp.

Since this implementation is a single function, it can be useful to be copied and pasted
into another script.

### Dependencies

- `openssl`
- `base32`
- `date` (`bash` < 5.0 only)

### Usage

Secret is a `base32` encoded string found in the field `secret` of a [totp url](https://github.com/google/google-authenticator/wiki/Key-Uri-Format),
usually 16 bytes long.

Using environment variable:

    OTP_SECRET_FILE=otp.secret ./otp

Using stdin:

    cat otp.secret | ./otp
    # or
    pass otp/secret | ./otp

The interval can be set too:

    OTP_SECRET_FILE=otp.secret OTP_INTERVAL=30 ./otp
