# Watson Assistant Classic docs

This repo contains the Markdown source files for the Watson Assistant Classic product documentation. This documentation is published [here](https://cloud.ibm.com/docs/services/assistant/index.html#about).

**Note:** These files document Watson Assistant instances using the "classic" or "old IA" interface. The documentation for the "New Watson Assistant" or "new IA" interface are [here](https://github.ibm.com/cloud-api-docs/watson-assistant).

These files are owned and maintained by the Watson Assistant documentation team:
- Craig Lordan (clordan@ibm.com)
- Robert Berry (rrberry@us.ibm.com)
- Charlotte Holt (charlotte.holt@ibm.com)

## Contributing

The `publish` branch of this repo reflects the current public documentation content. The `draft` branch is used for test builds and might contain backlevel or experimental versions of files. Do not create pull requests against the `draft` branch.

When changes are merged to the `publish` branch, they are automatically built and published. Consequently, changes cannot go into the `publish` branch until they have been reviewed, edited, and approved by a member of the documentation team.

If you want to submit updates or suggested changes, follow this process:

1. Create a new branch as a copy of the `publish` branch.

1. Make your updates in your new branch.

1. Create a pull request from your branch to `publish`.

    The documentation team will review your PR and might request changes. If the changes are relatively straightforward and self-contained, such as a corrected typo or a rewritten sentence, we will approve and merge it into `publish` (after any issues are addressed).

    If the changes are more extensive, such as a significant rewrite or entirely new content, the documentation team might need to make revisions for editorial or style reasons. In this case, we might open a new PR against your branch with our proposed revisions. You will then have the opportunity to review these revisions and incorporate the changes into your branch. Once the documentation team is satisfied with the proposed changes, we will merge your PR into `publish`.

**Note:** When changes are merged into the `publish` branch, the documentation is automatically rebuilt and published immediately. If the changes in your PR reflect changes that have not yet been released, make sure your PR is marked as a draft so that it will not be merged prematurely.
