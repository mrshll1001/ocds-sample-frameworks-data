# Sample OCDS data for framework call-offs

This repository contains sample data for how the various [stages of a framework agreement](http://standard.open-contracting.org/latest/en/implementation/related_processes) may appear in OCDS data. OCDS currently provides support for direct call-offs and mini-competitions in framework agreements, as well as Dynamic Purchasing Systems (DPS).

## Required extensions
Framework agreements represent many-to-many relationships between a variety of Buyers and Suppliers. This has implications for the structure of the OCDS data as `buyer` is declared at release level and `suppliers` are listed in `award/suppliers`; whereas each call-off from a Framework Agreement might involve a different buyer procuring from a different supplier. There are two extensions available to publishers to allow them to publish accurate Framework data as valid OCDS:

+ [Multiple Buyers - Contract Level Extension](https://extensions.open-contracting.org/en/extensions/contract_buyer/master/) adds a `buyer` reference field to the `Contract` block; allowing the buyer for each direct call-off to be associated with the purchase.
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

## Framework agreement within a single publisher
The following examples represent the creation of a framework by Glasgow City, who will be acting as the buyer and procuring entity. It also spans multiple [framework types](http://standard.open-contracting.org/latest/en/implementation/related_processes/) since it will be used for Multiple Suppliers with both direct call-offs and a mini-competition. Please note that in OCDS buyer and procuring entity may be different at times.

In these examples the publisher responsible is *Scottish Government* using their registered prefix of `ocds-r6ebe6`.

### Establishing the framework (Tender and Award)

Full examples:
+ [001_framework_set-up.json](/single_publisher/001_framework_set-up.json)
+ [008_framework_award.json](/single_publisher/008_framework_award.json)

The framework is established by first publishing a tender release opening up the procurement process as any normal contracting process published under OCDS.
> **Release Metadata**
> The tender release has the following metadata
```json
{
  "ocid": "ocds-r6ebe6-example_framework",
  "id": "ocds-r6ebe6-example_framework-tender-2019-01-01T00:00:00Z",
  "date": "2019-01-01T00:00:00Z",
  "tag": ["tender"],
  "initiationType": "tender"
}
```

Glasgow City is the one establishing the framework, so they have an entry in the `parties` array. They have the roles of `buyer` and `procuringEntity` as they are the ones procuring and calling-off from the framework agreemen:
> **Parties Array**
> The release has the following entries in the `parties` array at this time
```json
{
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
  "buyer": {
    "name": "Glasgow City",
    "id": "GB-LAS-GLG"
  }
}
```

The `tender` block is populated normally, with information about the framework tender. For frameworks, `tender/value` shoudl represent the total estimated upper value of the framework. Glasgow City is the procuring entity so they are referenced in `procuringEntity`.

> **Tender Block**
> The tender release has a populated `tender` block with the following information
```json
{
  "tender": {
    "id": "ocds-r6ebe6-example_framework-tender",
    "title": "An Example Framework",
    "description": "An Example Framework",
    "status": "active",
    "procuringEntity": {
      "name": "Glasgow City",
      "id": "GB-LAS-GLG"
    },
    "value": {
      "amount": "1000000",
      "currency": "GBP"
    }
  }
}

```

When a potential supplier bids for a position on the framework, they are added to the `parties` array with a role of *"tenderer"* since they have not yet been awarded the position.
> **Parties Array**
> Each tenderer's details are added to the `parties` array.
```json
{
  "numberOfTenderers": "6",
  "tenderers": [
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
    },
    {
      "name": "Supplier 4",
      "id": "GB-COH-00000001-supplier_60"
    },
    {
      "name": "Supplier 5",
      "id": "GB-COH-00000001-supplier_61"
    },
    {
      "name": "Supplier 6",
      "id": "GB-COH-00000001-supplier_62"
    }
  ]
}
```

Changes are also made in the `tender` block to add their reference to the list of tenderers, and update the total number of tenderers:

> **numberOfTenderers and Tenderers**
> numberOfTenderers and Tenderers are updated appropriately with the OrganizationReference
```json
{

  "numberOfTenderers": "1",
  "tenderers": [
    {
      "name": "Supplier 1",
      "id": "GB-COH-00000001-supplier_57"
    }
  ]
}



}
```

When a supplier is awarded a place on the framework, a release is made for the `award` award stage like a normal contracting process. The successful suppliers will be updated with the role of `supplier`. In this example Supplier 1, Supplier 2, and Supplier 3 have been awarded a position onto the framework.

> **Releasing an Award**
> An release is made adding the parties to the parties array
```json
{
  "ocid": "ocds-r6ebe6-example_framework",
  "id": "ocds-r6ebe6-example_framework-award-2019-02-01T00:00:00Z",
  "date": "2019-02-01T00:00:00Z",
  "tag": [
    "award"
  ],
  "initiationType": "tender",
  "parties": [
    {
      "name": "Glasgow City",
      "id": "GB-LAS-GLG",
      "identifer": {
        "scheme": "GB-LAS",
        "id": "GLG"
      },
      "roles": [
        "procuringEntity"
      ]
    },
    {
      "name": "Supplier 1",
      "id": "GB-COH-00000001-supplier_57",
      "identifer": {
        "scheme": "GB-COH",
        "id": "00000001"
      },
      "roles": [
        "tenderer",
        "supplier"
      ]
    },
    {
      "name": "Supplier 2",
      "id": "GB-COH-00000001-supplier_58",
      "identifer": {
        "scheme": "GB-COH",
        "id": "00000002"
      },
      "roles": [
        "tenderer",
        "supplier"
      ]
    },
    {
      "name": "Supplier 3",
      "id": "GB-COH-00000001-supplier_59",
      "identifer": {
        "scheme": "GB-COH",
        "id": "00000003"
      },
      "roles": [
        "tenderer",
        "supplier"
      ]
    }
  ]
}
```

An `awards` entry must also be published with the relevant information about the award, and references to the Suppliers are made in the `suppliers` array. Frameworks list all suppliers on a single award notice, with the `value` representing the total possible value of the framework and covering all suppliers with a place on it.
> **Award block**
> The award block is included in the release. It includes OrganizationReferences to the suppliers in the `suppliers` array and details of the award.
```json

  "awards": [
    {
      "id": "ocds-r6ebe6-example_framework-award-01",
      "title": "Award of suppliers on the example framework",
      "description": "Suppliers 1, 2, and 3 have been awarded a place on the framework",
      "status": "active",
      "date": "2019-02-01T00:00:00Z",
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
        "startDate": "2019-02-02",
        "endDate": "2020-01-31"
      }
    }
  ]
```

The framework is now established, and call-offs may now be made.

### Making direct call-offs (Contract)
Full examples:
+ [009_first_direct_calloff.json](/single_publisher/009_first_direct_calloff.json)
+ [010_second_direct_calloff.json](/single_publisher/010_second_direct_calloff.json)

A direct call-off from a framework agreement occurs when goods or services are procured directly from a supplier on an existing framework agreement without any further competition. For example a Framework may be established to supply an office with stationery and a direct call-off may be made to purchase items from this.

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

This is repeated for subsequent direct call-offs.

### Running a mini-competition (`relatedProcess`)
Full Example:
+ [011_mini-competition_tender.json](/single_publisher/011_mini-competition_tender.json)


Another form of calling off from a framework agreement is the *Mini-Competition*. In this form of purchase from the framework, a separate tendering process is run with participants limited to those already accepted as suppliers.

Representing a mini-competition in OCDS is straightforward. Broadly:
+ A *new contracting process* with a *new OCID* is created to represent the Mini Competition
+ In the new process the `relatedProcesses` array contains an entry referencing the OCID of the existing framework agreement
+ In the `tender` block of the new process, the `procurementMethod` is set to `limited` or `selective` to represent the fact that this was not an open tender.

> Note: This is a new contracting process where the buyer is known and the suppliers will be determined by the award block. Therefore the schema changes made by [Multiple Buyers - Contract Level Extension](https://extensions.open-contracting.org/en/extensions/contract_buyer/master/) and [Contract Suppliers Extension](https://extensions.open-contracting.org/en/extensions/contract_suppliers/master/) that apply to the Contract block may not be necessary for mini-competitions.

Following the previous sample of the Glasgow City framework agreement; after making their direct call-offs Glasgow City hold a mini-competition between suppliers on the framework. A new contracting process is created with an entry in `relatedProcesses` referencing the original framework agreement:

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

## Framework agreement across multiple publishers
Framework agreements may sometimes span data published by two or more different publishers. For example a framework agreement established and published by the UK National Government may be called off by buyers that are published by the Scottish Government or a regional publisher.

 There is very little difference in the OCDS representing a framework agreement handled by a single publisher, and a framework agreement with which multiple publishers interact. Since the OCID is globally unique it is used by both the publisher representing the framework setup and the publisher representing the call-offs from the framework.

In the following samples, the framework agreement is published by *Crown Commercial Services* using their registered prefix of `ocds-b5fd17`. The purchases from the framework are made by entities published by *Scottish Government* using their registered prefix of `ocds-r6ebe6`.

### Publisher 1 sets up the framework (Tender and Award)
Full Examples:
+ [012_two_publishers_framework_setup.json](/multi_publisher/012_two_publishers_framework_setup.json)
+ [013_two_publishers_framework_bids_added.json](/multi_publisher/013_two_publishers_framework_bids_added.json)
+ [014_two_publishers_framework_award.json](/multi_publisher/014_two_publishers_framework_award.json)

The first stages of the framework agreement are very similar to that when it only concerns a single publisher. In this sample, Crown Commercial Services commissions a framework agreement:
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
Bidders are added to the framework as per the previous example. For brevity this has been condensed to a single release:

> **Updates to the release**
> The tender release is updated to add bidders to the framework.
```json
{
  "ocid": "ocds-b5fd17-second_example_framework",
  "id": "ocds-b5fd17-second_example_framework-tenderUpdate-2019-03-31T00:00:00Z",
  "date": "2019-03-31T00:00:00Z",
  "tag": [
    "tenderUpdate"
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
    },
    {
      "name": "Supplier 1",
      "id": "GB-COH-00000001-supplier_57",
      "identifer": {
        "scheme": "GB-COH",
        "id": "00000001"
      },
      "roles": [
        "tenderer"
      ]
    },
    {
      "name": "Supplier 2",
      "id": "GB-COH-00000001-supplier_58",
      "identifer": {
        "scheme": "GB-COH",
        "id": "00000002"
      },
      "roles": [
        "tenderer"
      ]
    },
    {
      "name": "Supplier 3",
      "id": "GB-COH-00000001-supplier_59",
      "identifer": {
        "scheme": "GB-COH",
        "id": "00000003"
      },
      "roles": [
        "tenderer"
      ]
    },
    {
      "name": "Supplier 4",
      "id": "GB-COH-00000001-supplier_60",
      "identifer": {
        "scheme": "GB-COH",
        "id": "00000004"
      },
      "roles": [
        "tenderer"
      ]
    }
  ]
}
```
Remember to update the relevant fields in the `tender` block:
> **The Tender Block**
> The tender block is updated in the release to update the number of tenderers and reference them in the `tenderers` array.
```json
{
  "numberOfTenderers": "4",
  "tenderers": [
    {
      "name": "Supplier 1",
      "id": "GB-COH-00000001-supplier_57"
    },
    {
      "name": "Supplier 2",
      "id": "GB-COH-00000001-supplier_58"
    },
    {
      "name": "Supplier 3",
      "id": "GB-COH-00000001-supplier_59"
    },
    {
      "name": "Supplier 4",
      "id": "GB-COH-00000001-supplier_60"
    }
  ]
}
```

Next the framework agreement is finalised. Supplier 1, Supplier 2, and Supplier 3 have made it into the framework. Poor Supplier 4 was the only one excluded:
> Remember to update the entries under `release/parties` as well!

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

The framework is now established, and call-offs may be made from it.

### Buyers under a separate publisher make direct call-offs (Contract)
Full Examples:
+ [015_two_publishers_framework_first_direct-calloff.json](/multi_publisher/015_two_publishers_framework_first_direct-calloff.json)
+ [016_two_publishers_framework_second_direct-calloff.json](/multi_publisher/016_two_publishers_framework_second_direct-calloff.json)

With the framework agreement in place and published by *Crown Commercial Services*, it becomes straightforward to represent direct call-offs made by another publisher.

A direct call-off is represented by a `contract` block; so an OCDS release is published by *Scottish Government* containing the details of the Call-Off. Since it is published by *Scottish Government*, they use their registered prefix for the release id:
> **Release metadata**
> The direct call-off has the following metadata.
 ```json
 {
   "ocid": "ocds-b5fd17-second_example_framework",
   "id": "ocds-r6ebe6-second_example_framework-contract-2019-04-20 T00:00:00Z",
   "date": "2019-04-20T00:00:00Z",
   "tag": [
     "contract"
   ]
 }
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
        "procuringEntity"
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
> The contract section has the following information stored in `buyer` and `suppliers`.
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

### A buyer under a separate publisher runs a mini-competition (`relatedProcess`)
Full Example:
+ [017_two_publishers_framework_minicompetition_tender.json](/multi_publisher/017_two_publishers_framework_minicompetition_tender.json)

It is even more straightforward to run a mini-competition calling off of the framework. Since this is a new contracting process, a reference to the original Framework OCID should be included and the contracting process should proceed as a normal process under OCDS.

In this example, Edinburgh are running a mini-competition on the framework agreement established previously by *Crown Commercial Services*:
> **Tender Release Metadata**
> The tender release has the following data; note the entry in the `relatedProcesses` array.
```json
{
  "ocid": "ocds-r6ebe6-minicompetiion_from_other_publisher_framework",
  "id": "ocds-r6ebe6-minicompetiion_from_other_publisher_framework-tender-2019-05-01T00:00:00Z",
  "date": "2019-05-01T00:00:00Z",
  "tag": [
    "tender"
  ],
  "initiationType": "tender",
  "buyer": {
    "name": "Edinburgh",
    "id": "GB-LAS-EDH"
  },
  "parties": [
    {
      "name": "Edinburgh",
      "id": "GB-LAS-EDH",
      "identifer": {
        "scheme": "GB-LAS",
        "id": "EDH"
      },
      "roles": [
        "buyer",
        "procuringEntity"
      ]
    }
  ],
  "relatedProcesses": [
    {
      "id": "ocds-b5fd17-second_example_framework-parent",
      "relationship": "framework",
      "title": "An Example National Framework",
      "scheme": "ocid",
      "identifer": "ocds-b5fd17-second_example_framework",
      "uri": "https://example.org/records/ocds-r6ebe6-example_framework"
    }
  ]
}
```


## Dynamic Purchasing Systems
A Dynamic Purchasing System (DPS) is similar to a framework agreement with the exception that new suppliers may be awarded a position on the system at any time.

+ What implications does this have for our award model?
