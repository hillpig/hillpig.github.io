---
layout: post
title: Generate SSH Keys - Git
date: 2020-02-12
Author: 山猪
tags: [Git]
comments: true
---
![img](https://images.ctfassets.net/0lvk5dbamxpi/UTPF4vNZ56vh5Y9cndGfI/e6438d14fcdf045fe098702302456f07/SSH_Key_-_Authentication_Using_SSH_Keys)

<!-- more -->

* Generating an SSH key (Mac OS X / linux)

    1. Enter the following command in the Terminal window.

        ```console
            ssh-keygen -t rsa
        ```
    2. Press the ENTER key to accept the default location. The ssh-keygen utility prompts you for a passphrase.

    3. Type in a passphrase. You can also hit the ENTER key to accept the default (no passphrase). However, this is not recommended.
    Please note that you will need to enter the passphrase a second time to continue.

    After you confirm the passphrase, the system generates the key pair.

    ```console
        Your identification has been saved in /Users/user/.ssh/id_rsa.
        Your public key has been saved in /Users/user/.ssh/id_rsa.pub.
        The key fingerprint is:
        ae:89:72:0b:85:da:5a:f4:7c:1f:c2:43:fd:c6:44:38 user@mymac.local
        The key's randomart image is:
        +--[ RSA 2048]----+
        |                 |
        |         .       |
        |        E .      |
        |   .   . o       |
        |  o . . S .      |
        | + + o . +       |
        |. + o = o +      |
        | o...o * o       |
        |.  oo.o .        |
        +-----------------+
    ```
    Your private key is saved to the id_rsa file in the .ssh directory and is used to verify the public key you use belongs to the same cloud server. It's important to never share your private key with anyone, it is equivalent of your password!

    Your public key is saved to the id_rsa.pub file and it is the key you'll upload to our cloud service. You can save this key to the clipboard by running this:

    ```console
        pbcopy < ~/.ssh/id_rsa.pub
    ```
