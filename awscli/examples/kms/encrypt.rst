**Example 1: To encrypt the contents of a file on Linux or macOS**

The following ``encrypt`` command demonstrates the recommended way to encrypt data with the AWS CLI. ::

    aws kms encrypt \
        --key-id 1234abcd-12ab-34cd-56ef-1234567890ab \
        --plaintext fileb://ExamplePlaintextFile \
        --output text \
        --query CiphertextBlob | base64 \
        --decode > ExampleEncryptedFile

The command does several things:

#. Uses the ``fileb://`` prefix to specify the ``--plaintext`` parameter.

    The ``fileb://`` prefix instructs the CLI to read the data to encrypt, called the *plaintext*, from a file and pass the file's contents to the command's ``--plaintext`` parameter. If the file is not in the current directory, type the full path to file. For example: ``fileb:///var/tmp/ExamplePlaintextFile`` or ``fileb://C:\Temp\ExamplePlaintextFile``.

    For more information about reading AWS CLI parameter values from a file, see `Loading Parameters from a File <https://docs.aws.amazon.com/cli/latest/userguide/cli-using-param.html#cli-using-param-file>`_ in the *AWS Command Line Interface User Guide* and `Best Practices for Local File Parameters <https://blogs.aws.amazon.com/cli/post/TxLWWN1O25V1HE/Best-Practices-for-Local-File-Parameters>`_ on the AWS Command Line Tool Blog

#. Uses the ``--output`` and ``--query`` parameters to control the command's output.

    These parameters extract the encrypted data, called the *ciphertext*, from the command's output.

    For more information about controlling output, see `Controlling Command Output <https://docs.aws.amazon.com/cli/latest/userguide/controlling-output.html>`_ in the *AWS Command Line Interface User Guide*.

#. Uses the ``base64`` utility to decode the extracted output.

    This utility decodes the extracted ciphertext to binary data. The ciphertext that is returned by a successful ``encrypt`` command is base64-encoded text. You must decode this text before you can use the AWS CLI to decrypt it.

#. Saves the binary ciphertext to a file.

    The final part of the command (``> ExampleEncryptedFile``) saves the binary ciphertext to a file to make decryption easier. For an example command that uses the AWS CLI to decrypt data, see the `decrypt examples <decrypt.html#examples>`_.

**Example 2: Using the AWS CLI to encrypt data on Windows**

The preceding example assumes the ``base64`` utility is available, which is commonly the case on Linux and macOS. For the Windows command prompt, use ``certutil`` instead of ``base64``. This requires two commands, as shown in the following examples. ::

    aws kms encrypt \
        --key-id 1234abcd-12ab-34cd-56ef-1234567890ab \
        --plaintext fileb://ExamplePlaintextFile \
        --output text \
        --query CiphertextBlob > C:\Temp\ExampleEncryptedFile.base64

    certutil -decode C:\Temp\ExampleEncryptedFile.base64 C:\Temp\ExampleEncryptedFile

**Example 3: Encrypting with an asymmetric KMS key**

The following ``encrypt`` command shows how to encrypt plaintext with an asymmetric KMS key. The ``--encryption-algorithm`` parameter is required. ::

    aws kms encrypt \
        --key-id 1234abcd-12ab-34cd-56ef-1234567890ab \
        --encryption-algorithm RSAES_OAEP_SHA_256 \
        --plaintext fileb://ExamplePlaintextFile \
        --output text \
        --query CiphertextBlob | base64 \
        --decode > ExampleEncryptedFile

This command produces no output. The output from the ``decrypt`` command is base64-decoded and saved in a file.
