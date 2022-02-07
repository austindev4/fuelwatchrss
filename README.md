# fuelwatchrss
Source data from FuelWatch RSS feed (Western Australian Government) is transformed into dimension and fact tables.

## The ask
1. All resources should be deployed in Australia and with approriate tags.
2. Container in storage account (with Hierarchical namespace Enabled) to be named datalakestore.
3. Use the name dataLakeStorageAccountKey or similar as the secret name for datalakestore access key.
3. SQL PaaS database called DataMart of 20GB size.
4. Separate schemas for tmp and dw to host temporary data and curated data.

## The solution
This repository consists of Azure ARM templates. The [infra build template](https://github.com/austindev4/fuelwatchrss/blob/main/01%20-%20infra%20build%20template.json) deploys following resources in Azure Australia East region:
1. Database server
2. Database (DataMart)
3. Data factory
4. Storage account
5. Vault

The [data factory flows template](https://github.com/austindev4/fuelwatchrss/blob/main/02%20-%20data%20factory%20flows%20template.json) deploys flows to ADF that transform the raw feed into dimension and fact tables suitable for modelling.
Pass below parameters during datafactory flows ARM deployment.
1. Connection string for storage account
2. Connection string for DataMart

## Pending
Some tasks pending automation as below:-
Pre build:-
1. Create resource group in Australia East region.

Post build steps:-
1. Add secret for sqladmin to vault 
2. Obtain and add secret dataLakeStorageAccountKey from storage account to vault.
2. Add access policy (with secret get+list) in key vault for data factory principal id.
3. Create schemas for datamart using the [sql artifacts file](https://github.com/austindev4/fuelwatchrss/blob/main/03%20-%20SQL%20Artefacts.txt).

## Final state
Trigger the e2e pipeline on ADF. Flow is RSS->Blob->SQL.
![image](https://user-images.githubusercontent.com/62122629/152857171-59d72076-2900-4b14-8323-0486d16931e5.png)
