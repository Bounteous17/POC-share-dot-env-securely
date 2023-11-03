# POC-share-dot-env-securely

The `.env` files not only contain configuration parameters, in some cases they can contain __secrets__ as well. This is why it is __not recommended__ to **upload** this type of files to the **repositories**, even if it is private. This file can also coexist with others that can be called differently: `.env.development`, `.env.staging`, etc...

This time we are going to work with the `dotenv-vault` CLI to encrypt the content of our `.env` and upload the encrypted content to the repository.

```bash
npm install --save-dev dotenv-vault
```

In this next step we will generate the __private__ key to store the content of our beloved `.env` encrypted in the `.env.vault` file

```bash
npx dotenv-vault@latest local build
```

The previous command will have generated a file called `.env.keys` that will contain the encryption and decryption key. This key is rotating and is updated every time we launch the previous command.

You may have noticed that the `.env` file is not the only one that is ignored when new commits are made to the repository. Now `.env.keys` is also in the `.gitignore` file.

You are ready now for shareing the vault with your team. They would need to decrypt that now. Make sure your share the file `.env.keys` securely with them.
In any other way they would not be able to decrypt the content from `.env.vault`

Let's say that your key value is equal to `dotenv://:key_toor@dotenv.local/vault/.env.vault?environment=development`

```bash
npx dotenv-vault@latest local decrypt dotenv://:key_toor@dotenv.local/vault/.env.vault?environment=development
```

Congratulations! You have decrypted the content of the vault. That values can be stored now on a plain `.env` file for being used on your project.