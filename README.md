# dyDNS
[![Open in Code Ocean](https://codeocean.com/codeocean-assets/badge/open-in-code-ocean.svg)](https://codeocean.com/capsule/9669485/tree)

Cite: takefuji (2022) dyDNS for dynamic DNS updater in freedns.afraid.org [Source Code]. https://doi.org/10.24433/CO.2993693.v1

Y. Takefuji. (2023). International companies and governments need to be alerted to a fatal flaw in DOI management policy to prevent security breaches. Digital Society. https://doi.org/10.1007/s44206-023-00055-1

This is under review.

For software reproducibility validation, codeocean is used for generating DOI for publication in refereed journals (IEEE, Elsevier).

dyDNS is a PyPI package for periodically activating dynamicDNS in freedns.afraid.org.

Important information such as username, password and domain name should be encrypted 
with free dynamic DNS providers.

You need to have an account on freedns.afraid.org and choose dynamic domain names:
https://freedns.afraid.org/

You can have up to five domain names for free dynamic DNS.

dyDNS does not need superuser privilege.

# How to prepare for dyDNS
To use this PyPI dyDNS, you need to prepare for .freedns directory and two files: crypted and key.
<pre>
The following is the procedure for creating two files, crypted and key, in the .freedns directory:
1.You must create .freedns directory in your home directory.
$ cd
$ mkdir .freedns
$ cd .freedns
2.In .freedns directory, you should create a plain authentication file in plain text: freedns.info.
freedns.info file has three lines (username, password, dynamic domain name).
$ cat freedns.info
USERNAME = "username_freedns.afraid.org"
PASSWORD = "password_freedns.afraid.org"
UPDATE_DOMAINS = ["your_domain_name", ] 
3. Create a plain file of encryption/decryption key for OpenSSL.
key file must contain a string or strings with any characters.
$ cat key
your_key 
4. Change to the key file permission that only you can read.
$ chmod 700 key
5. Encrypt freedns.info to crypted file.
$ openssl enc -e -aes256 -pbkdf2 -in freedns.info -out crypted -k `cat key` 
6. Delete freedns.info file for security.
$ rm freedns.info
7. ".freedns" directory has two files: crypted and key
$ ls
crypted key
</pre>

# How to install dyDNS
$ pip install dydns

# How to run dyDNS
$ dydns

There are two types of messages from freedns.afraid.org:

b'No Updated: Address xxx.xxx.xxx.xxx has not changed.\n'

b'Updated Address xxx.xxx.xxx.xxx has changed.\n'


# crontab
You should type the following command to set crontab schedule:

$ crontab -e

The following shows an example in crontab scheduler to run dyDNS every 9 minutes.

*/9 * * * * dydns >/dev/null
