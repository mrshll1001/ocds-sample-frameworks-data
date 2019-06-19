Sample OCDS data for framework call-offs
========================================

This repository contains sample data for how the various [stages of a framework
agreement](http://standard.open-contracting.org/latest/en/implementation/related_processes) may appear in OCDS data. OCDS currently provides support for direct call-offs and mini-competitions in framework agreements.

This example will address the following scenarios:

1. A framework agreement established for a single buyer published by a single publisher.
2. A framework agreement established for multiple buyers, published by a single publisher.
3. A framework agreement where the OCDS data is published by multiple publishers. This may occur when one publisher establishes a framework agreement, and then other publishers release OCDS data representing the call-offs from this.

Types of Call-offs from Frameworks
----------------------------------
There are two types of call-offs made from framework agreement. These terms will occur throughout the guidance.

**A direct call-off** occurs when goods or services are procured directly from a supplier on an existing framework agreement without any further competition. For example a framework may be established to supply an office with stationery and a direct call-off may be made to purchase items from this.

**A mini-competition** is held when a buyer wants the suppliers on the framework to submit bids. It resembles a regular procurement process, with the key difference that the participants are limited to suppliers that have previously been awarded a position on the framework agreement.


Required extensions
-------------------
Framework agreements may represent many-to-many relationships between a variety of Buyers and Suppliers. This has implications for the structure of the OCDS data as `buyer` is declared at release level and `suppliers` are listed in `award/suppliers`; whereas each call-off from a Framework Agreement might involve a different buyer procuring from a different supplier. There are two extensions available to publishers to allow them to publish accurate Framework data as valid OCDS:

+ [Multiple Buyers - Contract Level Extension](https://extensions.open-contracting.org/en/extensions/contract_buyer/master/) adds a `buyer` reference field to the `Contract` block; allowing the buyer for each direct call-off to be associated with the purchase. This is only required if the framework agreement is established for the use of multiple buyers. If only a single buyer will be making call-offs, use `release/buyer` to model the buyer.
+ [Contract Suppliers Extension](https://extensions.open-contracting.org/en/extensions/contract_suppliers/master/) adds a `suppliers` array to the `Contract` block; allowing the supplier for each direct call-off to be associated with the purchase.

These should be declared appropriately in the package metadata:

```json
extensions_block.json
```

Framework agreement for a single publisher with multiple buyers
---------------------------------------------------------------
It is common for a framework agreement to be established by a large party such as a national government and then used for procurements by smaller parties underneath this such as local or municiple governments.

The following examples represent the creation of a framework agreement by the national-level Scottish Government for use by multiple local governments (called "Councils"). In this scenario, the publisher responsible is *Scottish Government* using their registered prefix of `ocds-r6ebe6`.

### Starting the process - publishing the tender
The framework is established by first publishing a tender release opening up the procurement process as any normal contracting process published under OCDS.

> **Release Metadata**
> The tender release has the following metadata

```
sample 001_framework_tender
```

Scottish Government is the one establishing the framework, so they have an entry in the `parties` array. They have the role of `procuringEntity` since they are the party establishing the framework.

```
sample 001_framework_tender
```

The `tender` block is populated normally, with information about the framework tender. For frameworks, `tender/value` should represent the total estimated upper value of the framework. Scottish Government is the procuring entity so they are referenced in `procuringEntity`.

> **Tender Block**
> The tender release has a populated `tender` block with the following information
```
sample 001_framework_tender
```

### Establishing the framework agreement - awarding suppliers a position on the framework
When a supplier is awarded a place on the framework, a release is made for the `award` award stage like a normal contracting process. The successful suppliers will be updated with the role of `supplier`. In this example Gamma Corp, Valkyrie Navigations, and Seaway Intelligence have each been awarded a position onto the framework.

> **Releasing an Award -- parties array**
> A release is made adding the parties to the parties array

```
sample 002_framework_award (parties array)
```

The release must also be published with the relevant information about the award by adding an entry to the `awards` array. In the `award` references to the Suppliers are made in the `suppliers` array. Frameworks list all suppliers on a single `award` notice, with the `value` representing the total possible value of the framework and covering all suppliers with a place on it.

> **Award block**
> The award block is included in the release. It includes OrganizationReferences to the suppliers in the `suppliers` array and details of the award.

```
sample 002_framework_award (award block)
```

The framework is now established, and call-offs may now be made.

### Making direct call-offs
In our scenario the framework agreement is established by Scottish Government and now Edinburgh make a direct call-off to procure items from Gamma Corp. A release is made with the appropriate release metadata:

> **Release metadata**
> The release for the direct call-off has the following metadata.

```
sample 003_first_call-off (metadata)
```

Edinburgh are a new party from the data's perspective. They are added to the `parties` array with the role of `buyer` (since their budget is being used to pay for goods).

```
sample 003_first_call-off (parties array)
```

A `contract` item is added to the `contracts` array with the details of the call-off, including the supplier and buyer information. This is where the [Multiple Buyers - Contract Level Extension](https://extensions.open-contracting.org/en/extensions/contract_buyer/master/) is used to reference the buyer using the `contract/buyer` field. Similarly, the [Contract Suppliers Extension](https://extensions.open-contracting.org/en/extensions/contract_suppliers/master/) is used to reference the specific supplier(s) involved in the call-off using `contract/suppliers`

```
sample 003_first_call-off (contract block)
```

For each subsequent call-off this process is repeated with a new release published to add the relevant buyer information to the `parties` array and populate a new item in the `contracts` array. A second example for a call-off can be found [here](/example_data/single_publisher/multi_buyer/004_second_call-off.json) wherein Glasgow City make a call-off from the same framework established in this scenario.

Framework agreement for a single publisher with a single buyer
--------------------------------------------------------------
It is also possible for a buyer to establish a framework for its own use. An example of this may be where a local government establishes a framework containing local suppliers. From the OCDS perspective this looks similar, but not identical, to the previous scenario with multiple buyers. Notably there is only one buyer; so the [Multiple Buyers - Contract Level Extension](https://extensions.open-contracting.org/en/extensions/contract_buyer/master/) is not required. The [Contract Suppliers Extension](https://extensions.open-contracting.org/en/extensions/contract_suppliers/master/) is still used because the direct call-offs from the framework will likely span several buyers.


The following examples represent the creation of a framework by Glasgow City for its own use. Glasgow will therefore be acting as the buyer and procuring entity.In these examples the publisher responsible is *Scottish Government* using their registered prefix of `ocds-r6ebe6`.

### Starting the process - publishing the tender
The same process is followed as before. The framework is put out to tender with a tender release in OCDS the same as any normal contracting process.

There is one key difference from the previous scenario in this step -- because the only party calling off from the framework will be Glasgow City they are referenced in the `release/buyer`

> **Tender Release**
> The tender release looks similar to the previous scenario, with the key difference of a `release/buyer` field.
```
001_framework_tender.json
```

The `tender` block is populated normally, with information about the framework tender. For frameworks, `tender/value` should represent the total estimated upper value of the framework. Glasgow City is the procuring entity so they are referenced in `procuringEntity`.

> **Tender Block**
> The tender release has a populated `tender` block with the following information

```
001_framework_tender.json
```
### Establishing the framework agreement - awarding suppliers a position on the framework

When a supplier is awarded a place on the framework, a release is made for the `award` award stage like a normal contracting process. The successful suppliers will be updated with the role of `supplier`. In this example Gamma Corp, Valkyrie Navigations, and Seaway Intelligence have been awarded a position onto the framework.

> **Releasing an Award**
> An award release is made adding the parties to the parties array

```
002_framework_award
```

Same as before, the `awards` entry must also be published with the relevant information about the award, and references to the Suppliers are made in the `suppliers` array. Remember to use a single award notice listing all suppliers. The `value` field should represent the total possible value of the framework.

> **Award block**
> The award block is included in the release. It includes OrganizationReferences to the suppliers in the `suppliers` array and details of the award.

```
002_framework_award
```

The framework is now established, and call-offs may now be made.

### Making direct call-offs (Contract)
Following the establishment of the framework agreement, Glasgow now make a direct call-off to Supplier 1. A release is made with the appropriate release metadata:
> **Release metadata**
> The release for the direct call-off has the following metadata.

```json
{
  "ocid": "ocds-r6ebe6-example_framework",
  "id": "ocds-r6ebe6-example_framework-contract-2019-03-01T00:00:00Z",
  "date": "2019-03-01T00:00:00Z",
  "tag": [
    "contract"
  ]
}
```

An item is added to the contracts array with the details of the call-off, including the supplier and buyer information:
> **Contracts Block**
> The release for the direct call-off has the following information in the contracts block. This framework only has a single buyer, so the `buyer` information does not need to be provided under `contracts/buyer`. Here, the Contracts Suppliers extension provides the `contracts/suppliers` array.

```json
{
  "contracts": [
    {
      "id": "ocds-r6ebe6-example_framework-contract-01",
      "awardID": "ocds-r6ebe6-example_framework-award-01",
      "title": "The First direct call-Off",
      "description": "A direct call off to buy things from Supplier 1 ",
      "suppliers": [
        {
          "name": "Supplier 1",
          "id": "GB-COH-00000001-supplier_57"
        }
      ]
    }
  ]
}
```

For each subsequent call-off a new item is added to the contracts array
and a release is published.

Framework agreement across multiple publishers
----------------------------------------------

Framework agreements may sometimes span data published by two or more different publishers. For example a framework agreement established and published by the UK National Government may be called off by buyers that are published by the Scottish Government or a regional publisher.

There is very little difference in the OCDS representing a framework agreement handled by a single publisher, and a framework agreement with which multiple publishers interact. Since the OCID is globally unique it is used by both the publisher representing the framework setup and the publisher representing the call-offs from the framework.

In the following samples, the framework agreement is published by *Crown Commercial Services* using their registered prefix of `ocds-b5fd17`. The purchases from the framework are made by entities published by *Scottish Government* using their registered prefix of `ocds-r6ebe6`.

### Considerations for integrating systems

To publish accurate OCDS data spanning multiple publishers, considerations must be made to integrate the data across multiple systems.

System integration should cover:
+ `ocid` - to link direct calls offs to framework establishments and to link mini-competitions to framework establishments
+ `award.id` - to link direct calls offs to framework establishments
+ `parties.id` - to keep consistent organisation identifiers between publishers
+ `contracts.id` - to avoid clashing contract ids for direct call-offs

### Publisher 1 sets up the framework (Tender and Award)

The first stages of the framework agreement are very similar to that when it only concerns a single publisher. In this sample, Crown Commercial Services establishes a framework agreement:

> **Release Metadata**
> The tender release has the following metadata.

```json
multi_publisher/001
```

They are the procuring entity of the framework so they are referenced under `tender/procuringEntity`:

> **Tender Block**
> The tender release has the following information in the `tender` block

```json
multi_publisher/001
```

Next the Suppliers are awarded a place on the framework agreement. Supplier 1, Supplier 2, and Supplier 3 have made it into the framework. The tenderers are included as part of the award release:

> **Awards**
> The awards release has the following information in `awards`.

```json
multi_publisher/001 (awards)
```

> Remember to update the entries under `release/parties` as well!

The framework is now established, and call-offs may be made from it.

### Buyers under a separate publisher make direct call-offs (Contract)
With the framework agreement in place and published by *Crown Commercial Services*, it becomes possible to represent direct call-offs made by another publisher.

A direct call-off is represented by a `contract` block; so an OCDS release is published by *Scottish Government* containing the details of the Call-Off. To guarantee its uniqueness *Scottish Government* preface the release ID with their registered prefix:
> **Release metadata**
> The direct call-off has the following metadata.

```json
multi_publisher/003
```

The buyer is also added to the `parties` array with the appropriate role, in this case *East Ayrshire*:

> **Parties Array**
> The contract release representing the direct call-off has the following update to the `parties` array.

 ```json
 multi_publisher/003 (parties)
 ```

As before the contract section refers back to the `awardID` of the framework agreement published by *Crown Commercial Services*. This will require access to the Award ID and the OCID of the framework agreement:

> **Contracts References**
> The contracts section refers back to the id of the award in `awardID`

```json
multi_publisher/003 (contracts.awardID)
```

Remember to include the `buyer` and `suppliers` in this section, added by the extensions used:
> **Buyer and Supplier In Contracts**
>  The contract section has the following information stored in `buyer` and `suppliers`. The ids used for each supplier will need to match between publisher systems -- following good practices around [organisation identifiers](http://standard.open-contracting.org/latest/en/schema/identifiers/#organization-ids) is recommended to assist in this.

```json
multi_publisher/003 (contract.buyer contract.suppliers)
```

Running a mini-competition using `relatedProcess`
-------------------------------------------------
Mini-competitions involve a further competitive stage and therefore are represented in OCDS using a separate contracting process which is linked to the related process that established the framework.

This is achieved through the following steps:
1. A *new contracting process* with a *new OCID* is created to represent the Mini-competition.
2. In the new process the `relatedProcesses` array contains an entry referencing the OCID of the existing framework agreement.
3. In the `tender` block of the new process, the `procurementMethod` is set to `limited` or `selective` to represent the fact that this was not an open tender.

> Note: This is a new contracting process where the buyer is known and the suppliers will be determined by the award block. Therefore the schema changes made by [Multiple Buyers - Contract Level Extension](https://extensions.open-contracting.org/en/extensions/contract_buyer/master/) and [Contract Suppliers Extension](https://extensions.open-contracting.org/en/extensions/contract_suppliers/master/) that apply to the Contract block are not necessary to model mini-competitions.

Following the previous example of the Glasgow City framework agreement; after making their direct call-offs Glasgow City hold a mini-competition between suppliers on the framework. A new contracting process is created with an entry in `relatedProcesses` referencing the original framework agreement:

> **Release Metadata**
> A release for a new contracting process is begun with the following details.

```json
example_minicompetition
```

Since this is a `tender` release the `tender` block contains information about the tender opportunity. The `procurementMethod` is set to `selective` to indicate that this is not an open tender.
> **Tender Block**
> The new contracting process' tender block contains the following information.

```json
example_minicompetition
```

From this point the contracting process continues as normal, with the award and contract stages being released under the new OCID created for the mini-competition.

Dynamic Purchasing Systems
--------------------------

A Dynamic Purchasing System (DPS) is similar to a framework agreement with the exception that new suppliers may be awarded a position on the system at any time.

-   What implications does this have for our award model?
-   Create a new award for each supplier awarded a space on the
    framework.
-   Add them to the original award for the set-up of the framework.
