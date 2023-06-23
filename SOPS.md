# SOPS - Secrets OPerationS

SOPS is an open-source tool developed by Mozilla that integrates with existing encryption systems like PGP (Pretty Good Privacy) or AWS Key Management Service (KMS). It allows you to encrypt secrets using a public key and then decrypt them using a private key. This encryption process ensures that secrets remain protected and can only be accessed by authorized individuals or systems.

### Downloading and installing sops
Im using windows so I needed to install Chocolatey first using:

```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
>> 
```

Then use Chocolatey to install sops:

```choco install sops```

```sops --version``` to check if its installed

### Create AWS KMS keys

Go to aws console and go to AMS KMS (Key Management Service) and create a key in one or two regions, I did two.

Copy the ARN of each key and paste them using this comma seperated command:

```
export SOPS_KMS_ARN="arn:aws:kms:eu-west-2:537561209985:key/mrk-84be7a983ce34043972280ee64ac9567, arn:aws:kms:eu-west-1:537561209985:key/mrk-9f08dc560e1f41e1ac10b521c9b5223c"
```

Next use your vim editor ``` vim sops.yaml``` and put your KMS ARNs in there.

```i``` To start writing

```
creation_rules:
  - kms: 'arn:aws:kms:eu-west-1:537561209985:key/mrk-9f08dc560e1f41e1ac10b521c9b5223c, arn:aws:kms:eu-west-2:537561209985:key/mrk-84be7a983ce34043972280ee64ac9567'

```

```:wq``` to save and exit


Then create your file to be encrypted ```sops test.yaml```

```
dev:
    password: my-secret-password!

```

Save and exit

Now run ```cat test.yaml``` and the contents of the file should be encrpted.

only peoople who have the Private key can decrypt it.

https://www.youtube.com/watch?v=DWzJ87KbwxA

### Decryption

