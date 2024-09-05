# ht

hardened tomb - use block device as tomb for secrets

USE AT YOUR OWN RISK

## install

place the file [`ht`](ht) under `PATH`

## tutorial

1. format block device

   ```
   ~ # # the default block device used is `/dev/vda`
   ~ # # since hardnedtomb is supposed to be in a virtualized environment
   ~ # # with a hardened minimal kernal and initrd without extra drivers
   ~ # # to change it, set the `HT_IMG` environment
   ~ # #
   ~ # # export HT_IMG=/dev/sdXY
   ~ # #
   ~ #
   ~ # # set the disk encryption passphrase during the execution
   ~ #
   ~ # ht disk

   WARNING!
   ========
   This will overwrite data on /dev/vda irrevocably.
   
   Are you sure? (Type 'yes' in capital letters): YES
   Enter passphrase for /dev/vda: 
   Verify passphrase
   ~ #
   ```

2. init hardened tomb

   ```
   ~ # # the tomb will be mounted to `HT_DIR` folder
   ~ # # by default `HT_DIR` is set to `/mnt`
   ~ # #
   ~ # # export HT_DIR=~/.ht
   ~ # #
   ~ # # also see `HT_MAP` and `HT_KEY` if you want to customize
   ~ #
   ~ # # disk encryption passphrase is required
   ~ # # then set the gpg passphrase during the execution
   ~ #
   ~ # ht init
   Enter passphrase for /dev/vda:

   ...

   Initialized empty Git repository in /mnt/.git/
   ```

3. insert some secrets

   ```
   ~ # ht addk SomeSecret
   enter secret in [ hex ]:

   ...

   ~ # # use `HT_FMT` to control the input format
   ~ #
   ~ # HT_FMT=txt ht addk SomeOtherSecret
   enter secret in [ txt ]:
   
   ...
   
   ```

4. lock hardened tomb

   ```
   ~ # ht lock
   ```

5. open hardened tomb

   ```
   ~ # ht open
   Enter passphrase for /dev/vda:

   ...
   
   ```
6. list secrets

   ```
   ~ # ht lstk
   |-- SomeOtherSecret
   `-- SomeSecret
   ```

7. show secrets

   ```
   ~ # ht getk SomeSecret
   a140bd507a57360e2fa503298c035854f0dcb248bedabbe7a14db3920aaacf57
   ~ #
   ~ # # use `HT_FMT` to control the output format
   ~ #
   ~ # HT_FMT=txt ht getk SomeOtherSecret
   oops
   ~ # HT_FMT=b64 ht getk SomeSecret
   oUC9UHpXNg4vpQMpjANYVPDcski+2rvnoU2zkgqqz1c=
   ~ #
   ~ # # or get the pem or der format of ecdsa secret key
   ~ # # note `HT_CRV` should be set correctly
   ~ #
   ~ # HT_FMT=pem ht getk SomeSecret
   -----BEGIN EC PRIVATE KEY-----
   MHQCAQEEIKFAvVB6VzYOL6UDKYwDWFTw3LJIvtq756FNs5IKqs9XoAcGBSuBBAAK
   oUQDQgAEIOptjOe8u0gzabKRHHXlYCo0KL5Elul/FK1S/UpqoONgg5xu2zIqIlV8
   cB7Q+h4Gz1dPvhe9aoVRacVllnLPqQ==
   -----END EC PRIVATE KEY-----
   ~ #
   ~ # # also able to request the signature
   ~ #
   ~ # echo "i hold the secret key" > message
   ~ # HT_FMT=sig ht getk SomeSecret message
   ~ # 3046022100f0d815849984c5bc06fd3d28e6ae8d6c0fedb1acb8a8970ead74878369e34492022100e2a6525dce1f55ffae88080d87e03feb7bc08ff9c667ff725bcf2b880056a842
   ```

## usage

read the source code to see more usage
