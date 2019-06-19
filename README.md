Sample OCDS data for framework call-offs
========================================

This repository contains sample data for how the various [stages of a framework
agreement](http://standard.open-contracting.org/latest/en/implementation/related_processes) may appear in OCDS data. OCDS currently provides support for direct
call-offs and mini-competitions in framework agreements, as well as Dynamic Purchasing Systems (DPS).

This example will address the following scenarios:

1. A framework agreement established for a single buyer published by a single publisher.
2. A framework agreement established for multiple buyers, published by a single publisher.
3. A framework agreement where the OCDS data is published by multiple publishers. This may occur when one publisher establishes a framework agreement, and then other publishers release OCDS data representing the call-offs from this.

Required extensions
-------------------

Framework agreements may represent many-to-many relationships between a variety of Buyers and Suppliers. This has implications for the structure of the OCDS data as `buyer` is declared at release level and `suppliers` are listed in `award/suppliers`; whereas each call-off from a Framework Agreement might involve a different buyer procuring from a different supplier. There are two extensions available to publishers to allow them to publish accurate Framework data as valid OCDS:

+ [Multiple Buyers - Contract Level Extension](https://extensions.open-contracting.org/en/extensions/contract_buyer/master/) adds a `buyer` reference field to the `Contract` block; allowing the buyer for each direct call-off to be associated with the purchase. This is only required if the framework agreement is established for the use of multiple buyers. If only a single buyer will be making call-offs, use `release/buyer` to model the buyer.
+ [Contract Suppliers Extension](https://extensions.open-contracting.org/en/extensions/contract_suppliers/master/) adds a `suppliers` array to the `Contract` block; allowing the supplier for each direct call-off to be associated with the purchase.

These should be declared appropriately in the package metadata:

```json
{
  "extensions": [
    "https://github.com/open-contracting/ocds_multiple_buyers_extension",
    "https://github.com/open-contracting-extensions/ocds_contract_suppliers_extension"
  ],
  "releases": []
}
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
A direct call-off occurs when goods or services are procured directly from a supplier on an existing framework agreement without any further competition. For example a framework may be established to supply an office with stationery and a direct call-off may be made to purchase items from this.

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
A direct call-off from a framework agreement occurs when goods or services are procured directly from a supplier on an existing framework agreement without any further competition. For example a framework may be established to supply an office with stationery and a direct call-off may be made to purchase items from this.

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

### Running a mini-competition (`relatedProcess`)


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
{
  "ocid": "ocds-b5fd17-second_example_framework",
  "id": "ocds-b5fd17-second_example_framework-tender-2019-03-01T00:00:00Z",
  "date": "2019-03-01T00:00:00Z",
  "tag": [
    "tender"
  ],
  "initiationType": "tender",
  "parties": [
    {
      "name": "Crown Commercial Services",
      "id": "GB-GOR-EA1015",
      "identifer": {
        "scheme": "GB-GOR",
        "id": "EA1015"
      },
      "roles": [
        "procuringEntity"
      ]
    }
  ]
}
```

They are the procuring entity of the framework so they are referenced under `tender/procuringEntity`:

> **Tender Block**
> The tender release has the following information in the `tender` block

```json
{
  "tender": {
    "id": "ocds-b5fd17-second_example_framework-tender",
    "title": "An Example National Framework",
    "description": "An Example National Framework",
    "status": "active",
    "procuringEntity": {
      "name": "Crown Commercial Services",
      "id": "GB-LAS-GLG"
    },
    "value": {
      "amount": "1000000",
      "currency": "GBP"
    }
  }
}
```

Next the Suppliers are awarded a place on the framework agreement. Supplier 1, Supplier 2, and Supplier 3 have made it into the framework. The tenderers are included as part of the award release:

> **Awards**
> The awards release has the following information in `awards`.

```json
{
  "awards": [
    {
      "id": "ocds-b5fd17-second_example_framework-award",
      "title": "Award of suppliers on the example framework",
      "description": "Suppliers 1, 2, and 3 have been awarded a place on the framework",
      "status": "active",
      "date": "2019-04-01T00:00:00Z",
      "value": {
        "amount": 1000000,
        "currency": "GBP"
      },
      "suppliers": [
        {
          "name": "Supplier 1",
          "id": "GB-COH-00000001-supplier_57"
        },
        {
          "name": "Supplier 2",
          "id": "GB-COH-00000002-supplier_58"
        },
        {
          "name": "Supplier 3",
          "id": "GB-COH-00000001-supplier_59"
        }
      ],
      "contractPeriod": {
        "startDate": "2019-04-01",
        "endDate": "2020-03-31"
      }
    }
  ]
}
```

> Remember to update the entries under `release/parties` as well!

The framework is now established, and call-offs may be made from it.

### Buyers under a separate publisher make direct call-offs (Contract)

With the framework agreement in place and published by *Crown Commercial Services*, it becomes possible to represent direct call-offs made by another publisher.

A direct call-off is represented by a `contract` block; so an OCDS release is published by *Scottish Government* containing the details of the Call-Off. To guarantee its uniqueness *Scottish Government* preface the release ID with their registered prefix:
> **Release metadata**
> The direct call-off has the following metadata.

```json
{    "ocid": "ocds-b5fd17-second_example_framework",    "id": "ocds-r6ebe6-second_example_framework-contract-2019-04-20 T00:00:00Z",    "date": "2019-04-20T00:00:00Z",    "tag": [      "contract"    ]  }
```

The buyer is also added to the `parties` array with the appropriate role, in this case *East Ayrshire*:

> **Parties Array**
> The contract release representing the direct call-off has the following update to the `parties` array.

 ```json
 {
 "parties": [
 {
 "name": "East Ayrshire",
 "id": "GB-LAS-EAY",
 "identifer": {
 "scheme": "GB-LAS",
 "id": "EAY"
 },
 "roles": [
 "buyer",
 ]
 }
 ]
 }
 ```

As before the contract section refers back to the `awardID` of the framework agreement published by *Crown Commercial Services*. This will require access to the Award ID and the OCID of the framework agreement:

> **Contracts References**
> The contracts section refers back to the id of the award in `awardID`

```json
{
  "contracts": [
    {
      "id": "ocds-r6ebe6-second_example_framework-contract-02",
      "awardID": "ocds-b5fd17-second_example_framework-award"
    }
  ]
}
```

Remember to include the `buyer` and `suppliers` in this section, added by the extensions used:
> **Buyer and Supplier In Contracts**
>  The contract section has the following information stored in `buyer` and `suppliers`. The ids used for each supplier will need to match between publisher systems -- following good practices around [organisation identifiers](http://standard.open-contracting.org/latest/en/schema/identifiers/#organization-ids) is recommended to assist in this.

```json
{
  "buyer": {
    "name": "East Ayrshire",
    "id": "GB-LAS-EAY"
  },
  "suppliers": [
    {
      "name": "Supplier 2",
      "id": "GB-COH-00000001-supplier_58"
    }
  ],
}
```



Running a mini-competition using `relatedProcess`
-------------------------------------------------
+ Running a mini-competition involves setting up a new contracting process and referring back to the original process
+ Therefore the method of modelling a mini-competition is the same regardless of how many publishers or buyers are involved.

+ Example data

Call-offs from a framework agreement can also be made via a mini-competition, where more than one supplier on the framework is invited to submit a bid to provide specific goods, works or services to a buyer.

Mini-competitions are represented in OCDS using a separate contracting process, linked to the establishment of the framework, because they involve a further competitive stage.

This is achieved through the following steps: + A *new contracting process* with a *new OCID* is created to represent the Mini Competition + In the new process the `relatedProcesses` array contains an entry referencing the OCID of the existing framework agreement + In the `tender` block of the new process, the `procurementMethod` is set to `limited` or `selective` to represent the fact that this was not an open tender.

> Note: This is a new contracting process where the buyer is known and the suppliers will be determined by the award block. Therefore the schema changes made by [Multiple Buyers - Contract Level Extension](https://extensions.open-contracting.org/en/extensions/contract_buyer/master/) and [Contract Suppliers Extension](https://extensions.open-contracting.org/en/extensions/contract_suppliers/master/) that apply to the Contract block are not necessary to model mini-competitions.

Following the previous example of the Glasgow City framework agreement; after making their direct call-offs Glasgow City hold a mini-competition between suppliers on the framework. A new contracting process is created with an entry in `relatedProcesses` referencing the original framework agreement:

> **Release Metadata**
> A release for a new contracting process is begun with the following details.

```json
{
  "ocid": "ocds-r6ebe6-example_framework-competition-01",
  "id": "ocds-r6ebe6-example_framework-competition-01-tender-2019-05-01T00:00:00Z",
  "date": "2019-05-01T00:00:00Z",
  "tag": [
    "tender"
  ],
  "initiationType": "tender",
  "buyer": {
    "name": "Glasgow City",
    "id": "GB-LAS-GLG"
  },
  "parties": [
    {
      "name": "Glasgow City",
      "id": "GB-LAS-GLG",
      "identifer": {
        "scheme": "GB-LAS",
        "id": "GLG"
      },
      "roles": [
        "buyer",
        "procuringEntity"
      ]
    }
  ],
  "relatedProcesses": [
    {
      "id": "ocds-r6ebe6-example_framework-parent",
      "relationship": "framework",
      "title": "An Example Framework",
      "scheme": "ocid",
      "identifer": "ocds-r6ebe6-example_framework",
      "uri": "https://example.org/records/ocds-r6ebe6-example_framework"
    }
  ],

}
```

Since this is a `tender` release the `tender` block contains information about the tender opportunity. The `procurementMethod` is set to `selective` to indicate that this is not an open tender.
> **Tender Block**
> The new contracting process' tender block contains the following information.

```json
"tender": {
  "id": "ocds-r6ebe6-example_framework-tender",
  "title": "An Example Mini Competition",
  "description": "A mini competition run off of the original framework",
  "status": "active",
  "procurementMethod": "selective",
  "procuringEntity": {
    "name": "Glasgow City",
    "id": "GB-LAS-GLG"
  },
  "value": {
    "amount": "2000",
    "currency": "GBP"
  }

}
```

From this point the contracting process continues as normal, with the award and contract stages being released under the new OCID created for the mini-competition.

Dynamic Purchasing Systems
--------------------------

A Dynamic Purchasing System (DPS) is similar to a framework agreement with the exception that new suppliers may be awarded a position on the system at any time.

-   What implications does this have for our award model?
-   Create a new award for each supplier awarded a space on the
    framework.
-   Add them to the original award for the set-up of the framework.
