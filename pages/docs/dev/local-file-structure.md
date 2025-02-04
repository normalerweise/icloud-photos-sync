# Local File Structure
This documentation describes the local file structure, that is written by this application. Even though the design of this application should handle alterations to the file system structure, it is not recommended to change it manually.

## Root folder
The root folder is specified through environment variable `DATA_DIR`. All assets and information are stored here:
  * `_All-Photos` Folder (aka. `ASSET_DIR`, [see src](https://github.com/steilerDev/icloud-photos-sync/blob/main/app/src/lib/photos-library/constants.ts))
  * `_Archive` Folder (aka. `ARCHIVE_DIR`, [see src](https://github.com/steilerDev/icloud-photos-sync/blob/main/app/src/lib/photos-library/constants.ts))
  * `.icloud-photos-sync.log`: Log file, overwritten upon application restart
  * `.trust-token.icloud`: The MFA trust token
  * User created folders from the iCloud Library

### Asset Dir
The asset dir contains all assets stored in the iCloud Photos Library. Their filename is the checksum, as provided by the iCloud backend, together with the correct file extension.

The m-time is set, based on the returned 'modified' timestamp.

### Archive Dir
If an archived folder is deleted in the iCloud backend, this folder will be moved to the archive dir. When archived folders are moved on the backend, a subfolder (`.stash`) is used to keep track of them. Files and folders in this directory are ignored by this application and can be organized in this folder as you wish.

### User Folders
Every user folder has two components:
  * A link with the display name of the folder/album, which links to
  * A folder containing the data, named after the UUID of the folder (and hidden: `.{UUID}`)

For each asset within an album, a link to the asset dir is created. Naming is based on the filename, provided by iCloud. Original files don't have a suffix, edits are suffixed with `-edited`, live photos are suffixed with `-live` ([see src](https://github.com/steilerDev/icloud-photos-sync/blob/main/app/src/lib/photos-library/model/asset.ts#L190)).

If a user folder contains a non-safe file (see [Safe Files](https://github.com/steilerDev/icloud-photos-sync/blob/main/app/src/lib/photos-library/constants.ts)), this marks the folder as archived, and its content is ignored upon future syncs.