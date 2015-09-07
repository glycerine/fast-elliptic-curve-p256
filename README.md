# fast-elliptic-curve-p256

https://blog.cloudflare.com/go-crypto-bridging-the-performance-gap/

Repackage the golang elliptic library enhancements by Vlad Krasnov and Shay Gueron as a standalone library.

This code offers substantial performance improvements for elliptic curve crypto.


References:

1. https://blog.cloudflare.com/go-crypto-bridging-the-performance-gap/
1. https://github.com/vkrasnov/openssl/blob/vlad/inv_ord/crypto/ec/asm/ecp_nistz256-x86_64.pl
1. https://go-review.googlesource.com/#/c/8968/
1. https://groups.google.com/forum/#!msg/golang-codereviews/m5QTnSUZU6c/Q5RUAdefWUwJ
1. https://github.com/openssl/openssl/pull/263/files
1. See the paper in doc/gueron_krasnov_fast_prime_field_elliptic_curve_cryptography_with_256-bit_primes_2013.pdf for the theory of operation.


Origin: Extracted from the following https://go.googlesource.com/go commit.

commit d23a3d36860a35d661845350768d2237d984cc75
Author: Vlad Krasnov <vlad@cloudflare.com>
Date:   Fri Apr 17 06:10:35 2015 -0700

    elliptic: assembly implementation of P256 for amd64

    This is based on the implementation used in OpenSSL, from a
        submission by Shay Gueron and myself. Besides using assembly,
            this implementation employs several optimizations described in:

        S.Gueron and V.Krasnov, "Fast prime field elliptic-curve
                                 cryptography with 256-bit primes"

    In addition a new and improved modular inverse modulo N is
        implemented here. Also included performance tweaks by Andy
            Polyakov from the OpenSSL team.

    The performance measured on a Haswell based Macbook Pro shows 21X
        speedup for the sign and 9X for the verify operations.
            The operation BaseMult is 30X faster (and the Diffie-Hellman/ECDSA
                key generation that use it are sped up as well).

    The adaptation to Go with the help of Filippo Valsorda

    Updated the submission for faster verify/ecdh, fixed some asm syntax and API

    Change-Id: I86a33636747d5c92f15e0c8344caa2e7e07e0028

