# Sending decomposed IMF Types
In order to send an Information Modelling Framework (IMF) Type that is built using references those references need to exist beforehand.

This gives us a prioritized list of kinds of types:
- RDL Term, used as Predicate in IMF Attribute Types.
- IMF Attribute Type, can be referenced from IMF Terminal Types and IMF Block Types.
- IMF Terminal Type can be referenced from IMF Block Types
- IMF Block Type, can be referenced from other IMF Block Types.

Given:
``` JSON
{
  
  "AspectName": "Function",
  "BlockName": "ElectroMotor",
  "Attributes": [
    {
      "AttNameNormalized": "Mass",
      "AttLabel": "Mass",
      "AttDescription": "Weight as specified in design",
      "Pca": "PCA_100003589",
      "IecCdd": "",
      "IecCddLink": "",
      "IecCddParent": null,
      "IecCddParentLink": null,
      "ScopeQualifier" : "imf:Design",
      "RegularityQualifier" : "",
      "RangeQualifier" : "imf:Nominal",
      "ProvenanceQualifier" : ""
    }
  ],
  "Terminals": [
    {
      "TerminalName": "ElectricalEnergyFlow",
      "Direction": "Input",
      "Attributes": [
        {
          "AttNameNormalized": "MinimumDesignVoltage",
          "AttLabel": "Minimum Design Voltage",
          "AttDescription": "Lower limit voltage",
          "Pca": "PCA_100003575",
          "IecCdd": "",
          "IecCddLink": "",
          "IecCddParent": null,
          "IecCddParentLink": null,
          "ScopeQualifier" : "imf:Design",
          "RegularityQualifier" : "",
          "RangeQualifier" : "Minimum",
          "ProvenanceQualifier" : ""
        },
        {
          "AttNameNormalized": "MaximumDesignVoltage",
          "AttLabel": "Maximum Design Voltage",
          "AttDescription": "Upper limit voltage",
          "Pca": "PCA_100003575",
          "IecCdd": "",
          "IecCddLink": "",
          "IecCddParent": null,
          "IecCddParentLink": null,
          "ScopeQualifier" : "imf:Design",
          "RegularityQualifier" : "",
          "RangeQualifier" : "imf:Maximum",
          "ProvenanceQualifier" : ""
        }
      ]
    },
    {
      "TerminalName": "MechanicalEnergyFlow",
      "Direction": "output",
      "Attributes": [
        {
          "AttNameNormalized": "imf:DesignContinuousMaximumOutput",
          "AttLabel": "Design Continuous Maximum Output",
          "AttDescription": "Designed Maximum continuous output",
          "Pca": "PCA_100003595",
          "IecCdd": "",
          "IecCddLink": "",
          "IecCddParent": null,
          "IecCddParentLink": null,
          "ScopeQualifier" : "imf:Design",
          "RegularityQualifier" : "imf:Continuous",
          "RangeQualifier" : "",
          "ProvenanceQualifier" : ""
        }
      ]
    }
  ]
}
```

Even though the IMF manual states a given Qualifier property's cardinality as 0-1, being precise by explicitly stating when there is no value is tidier than omitting it.

## Request / Response Sequence

When we want to create a compound type that references several other types we need to ensure that the referenced types are created in the order described above.

This gives us a flattened list of Types that we can then submit one by one.

In this example, we've defined all IMF attributes using established terms from the PLM-UOM ontology. This approach restricts expressiveness but provides a solid foundation to build upon.

### Request 1

``` JSON
{
    "author": {
        "email": "email",
        "name": "name"
    },
    "ChangeNote": "We need a new IMF-Attribute.",
    "NewAttributeSpecification": {
        "PrefLabel": "Mass",
        "Description": "Weight as specified in design",
        "Predicate": "PCA_100003589",
        "Uom": "PCA_100005429",
        "ScopeQualifier": "designQualifier",
        "RegularityQualifier": "",
        "RangeQualifier": "nominalQualifier",
        "ProvenanceQualifier": ""
    }
}
```
### Response
``` JSON
    {
        "CreatedAt": "https://rds.posccaesar.org/IMF/AttributeID4"
    }
```
### Request 2
``` JSON
{
    "author": {
        "email": "email",
        "name": "name"
    },
    "ChangeNote": "We need a new IMF-Attribute.",
    "NewAttributeSpecification": {
        "PrefLabel": "Minimum Design Voltage",
        "Description": "Lower limit voltage",
        "Predicate": "PCA_100003575",
        "Uom": "PCA_100003724",
        "ScopeQualifier": "designQualifier",
        "RegularityQualifier": "",
        "RangeQualifier": "minimumQualifier",
        "ProvenanceQualifier": ""
    }
    }
```

### Response 2
``` JSON
{
    "CreatedAt": "https://rds.posccaesar.org/IMF/AttributeID1"
}
```
### Request 3 
``` JSON
    {
        "author": {
            "email": "email",
            "name": "name"
        },
        "ChangeNote": "We need a new IMF-Attribute.",
        "NewAttributeSpecification": {
            "PrefLabel": "Maximum Design Voltage",
            "Description": "Upper limit voltage",
            "Predicate": "PCA_100003575",
            "Uom": "PCA_100003724",
            "ScopeQualifier": "designQualifier",
            "RegularityQualifier": "",
            "RangeQualifier": "maximumQualifier",
            "ProvenanceQualifier": ""
        }
    }
```
### Response 3
``` JSON
{
    "CreatedAt": "https://rds.posccaesar.org/IMF/AttributeID2"
}
```
### Request 4
``` JSON
{
    "author": {
        "email": "email",
        "name": "name"
    },
    "ChangeNote": "We need a new IMF-Attribute.",
    "NewAttributeSpecification": {
        "PrefLabel": "Design Continuous Maximum Output",
        "Description": "Designed Maximum continuous output",
        "Predicate": "PCA_100003595",
        "Uom": "PCA_100003725",
        "ScopeQualifier": "designQualifier",
        "RegularityQualifier": "",
        "RangeQualifier": "maximumQualifier",
        "ProvenanceQualifier": ""
    }
}
```
### Response 4
``` JSON
    {
        "CreatedAt": "https://rds.posccaesar.org/IMF/AttributeID3"
    }
```
### Request 5

Now we've submitted 4 Attributes and received 4 201 CreatedAt responses.
- https://rds.posccaesar.org/IMF/AttributeID1
- https://rds.posccaesar.org/IMF/AttributeID2
- https://rds.posccaesar.org/IMF/AttributeID3
- https://rds.posccaesar.org/IMF/AttributeID4

Moving on to Terminal types we then use the relevant URI's from the CreatedAt responses instead of reiterating the attribute definitions.


Note the usage of  "https://rds.posccaesar.org/IMF/AttributeID3" in the content below
``` JSON

{
    "author": {
        "email": "email",
        "name": "name"
    },
    "ChangeNote": "We need a new IMF-Terminal.",
    "NewTerminalSpecification": {
        "PrefLabel": "MechanicalEnergyFlow",
        "Direction": "output",
        "Attributes" : [
            "https://rds.posccaesar.org/IMF/AttributeID3"
        ]
    }
}
``` 


### Response 5
``` JSON
{
    "CreatedAt": "https://rds.posccaesar.org/IMF/Terminal1"
}
```


### Request 6
``` JSON
{
    "author": {
        "email": "email",
        "name": "name"
    },
    "ChangeNote": "We need a new IMF-Terminal.",
    "NewTerminalSpecification": {
        "PrefLabel": "ElectricalEnergyFlow",
        "Direction": "Input",
        "Attributes" : [
            "https://rds.posccaesar.org/IMF/AttributeID1",
            "https://rds.posccaesar.org/IMF/AttributeID2"
        ]
    }
}
``` 
### Response 6
``` JSON
{
    "CreatedAt": "https://rds.posccaesar.org/IMF/Terminal2"
}
```
### Request 7

Finally when we get to the Block Type we apply the same approach that we just did for the Terminal Types, and refer to the returned URI's again.

Note that both the Attributes and Terminals arrays below contain only URI's.
``` JSON
{
    "author": {
        "email": "email",
        "name": "name"
    },
    "ChangeNote": "We need a new IMF-Block.",
    "NewTerminalSpecification": {
        "PrefLabel": "ElectroMotor",
        "AspectName": "Function",
        "Attributes" : [
            "https://rds.posccaesar.org/IMF/AttributeID4"
        ],
        "Terminal" : [
            "https://rds.posccaesar.org/IMF/Terminal1",
            "https://rds.posccaesar.org/IMF/Terminal2"
        ]
    }
}
```
### Response 7
``` JSON
{
    "CreatedAt": "https://rds.posccaesar.org/IMF/<Block1>"
}
```

This approach enables us to coin and manage new ID's for suggested Types at a quick rate. An application that has access to the initial data model above can implement a simple approach to flatten and send a large model automatically without unreasonable demands on an end user.


### Addendum Qualifier values
``` JSON
{
    "regularity" : [
        "imf:absolute",
        "imf:continuous"
    ],
    "range" : [
        "imf:average",
        "imf:maximum",
        "imf:minimum",
        "imf:nominal",
        "imf:normal"
    ],
    "provenance" : [
        "imf:calculated",
        "imf:measured",
        "imf:specified"
    ],
    "scope" : [
        "imf:design",
        "imf:operating"
    ]
}
```