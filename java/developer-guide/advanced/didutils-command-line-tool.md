# Didutils Command Line Tool

In addition to SDK and sample code, DID’s source code repository also contains a command line tool related to DID, which provides a command set commonly utilized in daily use of DID.

The release of the DID SDK already contains the pre-compiled jar package (the didutils.jar) which can be downloaded from the [Assets of the latest release page](https://github.com/elastos/Elastos.DID.Java.SDK/releases/latest).

After downloading, the tool can be used through the following command:

```bash
java -jar didutils.jar [COMMAND] <ARGS>
```

A command list can be supported through the following commands:

```bash
java -jar didutils.jar --help
```

## Supported Command List       &#x20;

| 命令 Command     | 描述Description                          |
| -------------- | -------------------------------------- |
| resolvedid     | Resolve DID from the ID side chain.    |
| verifydoc      | Verify the DID document.               |
| verifyvc       | Verify the verifiable credential.      |
| verifyvp       | Verify the verifiable presentation.    |
| verifyjwt      | Verify the JTW/JWS token.              |
| createidentity | Create a RootIdentity.                 |
| listidentity   | List the RootIdentities.               |
| deleteidentity | Delete the root identity.              |
| createdid      | Create a DID.                          |
| listdid        | List the DIDs.                         |
| deletedid      | Delete the DID.                        |
| displaydid     | Display the DID document and metadata. |
| publishdid     | Publish DID to ID transaction payload. |
| recover        | Recover the wrong DIDs.                |
| simchain       | Simulated ID Chain for testing.        |
| jsonld         | JSON-LD tools.                         |
| shell          | Interactive shell.                     |

## Get Help from Specific Commands

Each of the above commands can support the --help command line parameter to receive help from the command, such as getting help from the resolvedid command:

```bash
java -jar didutils.jar resolvedid --help
```
