[TOC]

# SSH

## Theoretical knowledge

- SSH (Secure Shell) is a secure protocol.
- TCP port 22
- both symmetric and asymmetric encryption are used in SSH.
- **symmetric encryption**: the same secret key is used by the sender to encrypt and the receiver to decrypt the message. SSH creates and shares a session key (aka shared key) using a "**key exchange algorithm**," which is secure because **it never transmits the key between the two hosts**. Instead, both hosts share public pieces of data and construct the secret key independently.
- **asymmetric encryption**: for authentication. Asymmetric encryption uses a key-pair (**a public key and a private key**) to encrypt and decrypt data.  They are generated at the same time but cannot be derived one from the other. By default, the key pair will be stored in the **~/.ssh/** directory. The sender and receiver will share the public key, then sender encrypt the plain text by receiver's public key and the receiver will decrypt it through receiver's private key.
- `RSA (originated by R. Rivest, A. Shamir, L. Adleman)` and `DSA (Digital Signature Algorithm)` are two encryption algorithms that can be employed during public/private keypair generation.
  - `RSA` is the preferred algorithm. Different key length are possible, with a minimum of 1024 bits and a recommended length of 2048 bits.
  - `DSA` is an older algorithm and is no longer recommended. A key length of 1024 bits is typical for this algorithm.

## Lab and Practice

### generate the SSH key pair

```shell
ssh-keygen -t rsa -b 4096
```

you will get two file, the suffix of the public key is `.pub`.



# AWK

## Theoretical Knowledge

- Awk is a language that is used for finding patterns
- no require to compile
- Awk splits the input file into "**records**" and then splits each record into "**fields**." By default, each line break is a record separator and each space or tab is a field separator. 

## Lab Practice

### general syntax

```shell
awk options ‘selection-criteria {action}’ input_file > output_file
```

Within each record (think line), the first field (think word) is denoted by $1, the second field is denoted by $2. $0 refers to the entire record.

### print

- print the 1st word of each line

  ```shell
  awk '{print $1}' input_file
  ```

- to print the second word alongside the first

  ```shell
  awk '{print $1 "-" $2}' sample.txt
  ```

- to print first two words of each line

  ```shell
  awk '{print $1 $2}' sample.txt
  ```

  



### filter

- filter the line with more than 20 characters

  ```shell
  awk 'length($0) > 20' input_file
  ```

- filter the line contains "ubuntu@node-1", this is a regular expression

  ```shell
  awk "/ubuntu@node-1/" running-config.txt
  ```

  

### build-in features

- **NF**: number of fields in the current record.

- **FIELDWIDTHS**:

  ```shell
  $ echo "aaabbbbcccccdddddd" | awk -v FIELDWIDTHS="3 4 5 6" '{for(i=1;i<=NF;i++)print $i}'
  aaa
  bbbb
  ccccc
  dddddd
  ```

  - `-v`: variable

- **FS**: field separator

  ```shell
  $ echo "John Q. Smith, 29 Oak St." | awk 'BEGIN { FS = "," } ; { print $2 }'
  29 Oak St.
  ```

- **OFS**: Output Field Separator.

  ```shell
  $ echo "1+3 2+2"| awk 'BEGIN{OFS="=";} {print $1,$2;}'
  1+3 = 2+2
  ```

- **ORS:** Specifies the Output Record Separator. `Awk` generally sees each line in a text file as a new record, and each record contains a series of fields.

  ```shell
  $ awk 'BEGIN { ORS = ";" }{print $0}' sample.txt
  This is a sample;This is a second sample
  ```

  

# SED

## Theoretical knowledge

### start

a stream editor in Linux.  It takes an input stream (**a file or input from a pipeline**) and allows you to perform text manipulation operations like finding, filtering, replacement, insertion, deletion and regular expressions.

## Lab and Practice

### general syntax

```shell
sed OPTIONS... [SCRIPT] [INPUTFILE...]
```

### replace

- Replace all occurrences of "unix" with "linux" in the input file "input.txt"

  ```shell
  sed -i 's/unix/linux/g' input.txt
  ```

### delete

- Delete the 2nd line from the file input.txt

  ```shell
  sed -i '2d' input.txt
  ```



# GREP

## Lab and Practice

### filter

- select another 5 lines after 'interface'

  ```shell
  grep -A 5 'interface' running-config.txt
  ```

