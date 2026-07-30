# apic-studio-idig-test

The Apic IDIG Test repo allows you to test the IDIG Broker action from [here](https://github.com/ibm-apiconnect/apic-studio-idig-action). This is an example repository from where the API Studio projects can be published and deleted to the API connect IDIG Broker using the IDIG broker action.

See [idig_publish.yml](.github/workflows/idig_publish.yml)
The file `idig_publish.yml` has an example of publishing new and modified API Studio projects to the IDIG broker host `t-us00.apiconnect.test.automation.ibm.com` when any change is committed to the existing projects in the repository or when a new API Connect Studio project is created.<br />

The job `discover-changes` checks for the changes in the project folders of the repository and detects any newly created API Studio projects.<br /> 
The `publish-changes` job uses the apic-studio-idig-action repository main branch in the ibm-apiconnect organization
 - uses: ibm-apiconnect/apic-studio-idig-action@main <br /> 
with the parameters supplied
```
with:
  idig_host: ${{ env.IDIG_BROKER_HOST }}
  platform_idig_prefix: ${{ env.PLATFORM_IDIG_PREFIX }}
  changed_files: ${{ needs.discover-changes.outputs.changed_files }}
  deleted_files_content: ${{ needs.discover-changes.outputs.deleted_files_content }}
  insecure_skip_tls_verify: 'true'
  auth_username: ${{ secrets.IDIG_USERNAME }}
  auth_password: ${{ secrets.IDIG_PASSWORD }}
```

## Authentication Setup
**Important:** You must set up the secret for the Github Action as described in the steps [here](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets#creating-encrypted-secrets-for-a-repository). The secret name for the `username` authentication should be `IDIG_USERNAME` and the secret name for the password should be `IDIG_PASSWORD`. The secrets can be obtained from the list of secrets in your openshift cluster. The secret should be present in the same namespace as your IDIG broker deployment. The username and password data as part of the secret should be decoded first from base64 before adding them to your Github action repository.

**Note:** If your commit contains many new projects or changes to existing projects, there may be some delay publishing and updating data contained within the IDIG broker. 
