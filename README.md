# SOPS Demo

A simple demo of [getsops/sops](https://github.com/getsops/sops). 

This is for a talk I'm giving at [the Denver Platform Engineering Meetup](https://www.meetup.com/platform-engineering-denver/events/298484340/) on 02/07/24.

## Installing SOPS

To install SOPS, you have two options:

1. Use `aqua` to install (Recommended because aqua is another great tool that I could give an entire talk on):
   1. [Install aqua via the instructions on their site](https://aquaproj.github.io/docs/install).
   2. Once `aqua` is installed, run `aqua install` at the root of the directory.
2. [You can install SOPS directly from their releases page](https://github.com/getsops/sops/releases).

## Using SOPS Yourself

### AWS Secrets

For the AWS Secrets file, you can't unfortunately 😅

The whole point to utilizing `sops` for the "secret" values that we are storing in `secrets/aws-secrets.yaml` is that you would need to have access to my AWS Account's KMS key to be able to decrypt those secrets. So if you try to run `sops secrets/aws-secrets.yaml` like I do during the demo, this will fail because you don't have access to the AWS KMS key that I used to create that file and therefore the tool can't decrypt the secret values.

If you want to bring your own AWS KMS key and use that with SOPS, you can go ahead and do that by running the following:

```bash
sops --kms $YOUR_KMS_KEY_ARN secrets/my-own-aws-secrets.yaml
```

### age Secrets

[`age`](https://github.com/FiloSottile/age) is simple, modern and secure file encryption tool, format, and Go library.

We **can** use `age` to allow you to demo the `secrets/age-secrets.yaml` file locally! This is because `age` works with simple public / private keys and we've checked the private key into `./key.txt`, so that you can use it locally.

To demo / edit the `secrets/age-secrets.yaml` file, you don't even need `age` installed! You just need to run the following from the root of this project:

```bash
SOPS_AGE_KEY_FILE=key.txt sops secrets/age-secrets.yaml
```

That will open a new editor for you where you can edit the decrypted values and upon exiting that editor, it will decrypt them with the `age` key 🎉
