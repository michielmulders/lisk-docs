# Lisk Commander Sensitive Inputs

Sometimes Lisk Commander requires sensitive inputs such as passphrases or secret messages, which you may not want to pass directly as command line arguments because they will made available to e.g. your bash history or process managers. 
This page describes the various options you have for securely passing sensitive inputs to Lisk Commander commands. 
The format for providing Lisk Commander with sensitive inputs is loosely based on the format used by OpenSSL.

We shall use the `message:encrypt` command for the examples with a secret passphrase as a sensitive input, but the same pattern applies to other commands which accept sensitive inputs as well.

## Via a password prompt

If a passphrase is required for a command, and no source is provided via the `--passphrase` option, Lisk Commander will prompt you for a password input so you can type your passphrase in directly. 
Note this approach is only available for passphrases, not sensitive inputs of other sorts.

```bash
$ lisk message:encrypt bba7e2e6a4639c431b68e31115a71ffefcb4e025a4d1656405dfdcd8384719e0 'Hello world'
Please enter your secret passphrase: ****************************************************************
Please re-enter your secret passphrase: ****************************************************************
```

In the case of commands which may or may not involve a second passphrase, a prompt can be triggered using `--second-passphrase` prompt:

```bash
$ lisk transaction:create --type=0 100 9397838105554816361L --second-passphrase prompt
Please enter your secret passphrase: ****************************************************************
Please re-enter your secret passphrase: ****************************************************************
Please enter your second secret passphrase: ****************************************************************
Please re-enter your second secret passphrase: ****************************************************************
```

## Via an environmental variable

If you have stored your passphrase in an environmental variable, you can specify the variable name using the --passphrase option. 
Note this approach is only available for passphrases, not sensitive inputs of other sorts.

```bash
 lisk message:encrypt bba7e2e6a4639c431b68e31115a71ffefcb4e025a4d1656405dfdcd8384719e0 'Hello world' --passphrase env:PASSPHRASE
```

## Via a file

If you have stored your passphrase in a file, you can specify the path using the --passphrase option, and Lisk Commander will read the passphrase from the first line. 
If you have stored data of another sort in a file, you can specify the path using the --data option, and Lisk Commander will read the entire contents of the file.

```bash
 lisk message:encrypt bba7e2e6a4639c431b68e31115a71ffefcb4e025a4d1656405dfdcd8384719e0 --data file:./secret_data.txt --passphrase file:./passphrase.txt
```

## Via stdin

Lisk Commander will read a passphrase from the first line of stdin via the --passphrase option, or multiple lines for data of other sorts via the --data option. 
If stdin is specified as the source of both the passphrase and some other data, then the passphrase is assumed to be the first line, and the data to follow. 
Note the actual use of echo with plaintext strings as in the below example is **not recommended**.

```bash
 $ echo 'my secret passphrase on the first line\Followed by\nsome data that\nspans multiple lines' | lisk message:encrypt bba7e2e6a4639c431b68e31115a71ffefcb4e025a4d1656405dfdcd8384719e0 --data stdin --passphrase stdin
```

## Via a plaintext option

For convenience, Lisk Commander also supports providing a plaintext passphrase via the --passphrase option. 
This can be useful for prototyping or testing (for example on the testnet) but **should never be used when security is a concern**. 
Note this approach is only available for passphrases, not sensitive inputs of other sorts.

```bash
 lisk message:encrypt bba7e2e6a4639c431b68e31115a71ffefcb4e025a4d1656405dfdcd8384719e0 'Hello world' --passphrase 'pass:my secret passphrase'
```
