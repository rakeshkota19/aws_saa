
# Versioning

- [Versioning](#versioning)
  - [Versioning](#versioning-1)
  - [Disadvantages](#disadvantages)
  - [MFA delete](#mfa-delete)

## Versioning

- Versioning for bucket is disabled initially
- You can enable it, but once enable you can't disable and only suspend it
- After suspension, we can go to enable and vice versa.
- Versioning, disabled means all the id's for each object are null.
- If versioning is enabled, for a new edit, id will be updated with the same key name. 
- If object is accessedd without any id, latest/current version will be returned.
- We can also get a specific version in the get request
- Delete a object and don't provide an id, s3 will add a delete marker which hides all the versions for us.
- We can also delete a delete marker, and all the versions will be visible.
- However we can delete any version by providing the id, if deleted recent one, then the next one will be the recent bucket


## Disadvantages

- Cannot be switched off
- Space is consumed by all versions
- Even suspended, previous contains all versions
- Charges for all

## MFA delete

- Enabled in versioning configuration
- MFA is required to change bucket versioning state
- MFA is required to delete versions
- Serial number (MFA) + code passed with API calls to interact versioning