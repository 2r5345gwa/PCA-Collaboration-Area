# Block with hasPart "Casing" which has height Vs Block with Casing_Height directly

At one point we will have to capture that there is a type of equipment that has an important part. The important part again has some important feature.

For example a Temperature Transmitter that has a Casing and the Casing has height, length and width.

In the examples we create in this document the URLs used will have short descriptive names in order to ensure readability of the examples. For example, the Subject IRI in the triple
``` 
imf:Length a imf:Attribute
```
Will not be imf:Length when stored in the new PCA platform.

# The hasPart approach
```
:TemperatureTransmitterBlock a TemperatureTransmitterBlockType ;
        imf:hasPart :CasingBlock .

:CasingBlock a CasingBlockType ;
    imf:hasHeight "10^^xmlns:int" ;
    imf:hasWidth "10^^xmlns:int" ;
    imf:hasLength "10^^xmlns:int" .
```
Working with the above example we need two IMF block types and 3 IMF Attributes.

```
:TemperatureTransmitterBlockType a imf:BlockType, sh:NodeShape;
    skos:preflabel "Temperature transmitter block type" ;
    rdfs:subClassOf imf:Block ;
    sh:property [
        sh:path imf:hasPart ;
        sh:node imf:CasingBlockType 
    ] ;

:CasingBlockType a imf:BlockType, sh:NodeShape;
    skos:prefLabel "Casing block type" ;
    rdfs:subClassOf imf:Block ;
    sh:property [
        sh:path imf:hasHeight
        sh:datatype xsd:double 
    ] ;
    sh:property [
        sh:path imf:hasLength
        sh:datatype xsd:double 
    ]
    sh:property [
        sh:path imf:hasWidth
        sh:datatype xsd:double 
    ] .
```

The Blocks can be defined using existing reference data for equipment types for Temperature Transmitter and Casing.
The Attributes can be defines using existing physical qualities for height, width and length.

The specification for one of the attributes will be revisited alongside one of the attribute specifications from the casing_Length approach later.

# The Casing_Length approach
```
:TemperatureTransmitterBlock a TemperatureTransmitterBlockType ;
    imf:hasCasingHeight "10^^xmlns:int" ;
    imf:hasCasingWidth "10^^xmlns:int" ;
    imf:hasCasingLength "10^^xmlns:int" .
```

Now we only need to define 1 block and we still have the equipment type for TemperatureTransmitter to base it on.
We still need 3 Attributes. 

```
:TemperatureTransmitterBlockType a imf:BlockType, sh:NodeShape;
    skos:preflabel "Temperature transmitter block type" ;
    rdfs:subClassOf imf:Block ;
    sh:property [
        sh:path imf:hasCasing_Height
        sh:datatype xsd:double 
    ] ;
    sh:property [
        sh:path imf:hasCasing_Length
        sh:datatype xsd:double 
    ]
    sh:property [
        sh:path imf:hasCasing_Width
        sh:datatype xsd:double 
    ] .
```
in order to be able to differentiate between imf:hasCasingHeight from imf:hasHeight (Which is likely going to be needed elsewhere) we need to base it on a different point of reference data than imf:hasHeight. There isn't a suitable term that sufficiently captures the needed semantic definition of "The height of the casing that is a part of this other equipment", so we need to make one. The would be true for length and height.

# Discussion
Lets expand on the shortcuts for the attributes for Height used in the blocks above.

If we start out with imf:hasLength.

```
imf:hasLength a imf:Attribute ;
    imf:Predicate <http://rds.posccaesar.org/ontology/plm/rdl/PCA_100003585> ;
    imf:hasQualifier imf:Design
```
This is a minimal definition of an IMF Attribute intended to capture the design height of "something", without specifying a unit of measure.

Lets move on to imf:hasCasing_Length.
If we go with the immediate interpretation of what the author wants to convey we get the following
```
imf:hasCasing_Length a imf:Attribute ;
    imf:Predicate <http://rds.posccaesar.org/ontology/plm/rdl/PCA_100003585> ;
    imf:hasQualifier imf:Design
```

Looks OK, the only problem is that we already have an attribute that captures the exact same concept in imf:hasLength. The IRI in the subject field of the triples has no impact of meaning of the triples, only what they are about. So, due to the way IMF Attributes are defined we can conclude that they are the same thing.

Let's try again.

We certain that we want to use the design qualifier, and we're also certain that we do not want to use any of the other qualifiers. Which means that we're left with only the Object in the triple 
```
imf:hasCasing_Length imf:Predicate <http://rds.posccaesar.org/ontology/plm/rdl/PCA_100003585> 
```

Then we're off to find a RDL term that captures the idea of "The length of the casing of a 'thing'".

Maybe we want to create a subClass of http://rds.posccaesar.org/ontology/plm/rdl/PCA_100003585 aka Length? 

Specifically for the length of a https://staging3.data.posccaesar.org/rdl/RDS13029297 aka Casing when the Casing is a part of a https://rds.posccaesar.org/ontology/lis14/rdl/PhysicalObject/?


```
rdl:Casing_Length rdf:type owl:Class ;
                  rdfs:subClassOf lis:PhysicalQuantity ,
                                  [ rdf:type owl:Restriction ;
                                    owl:onProperty lis:qualityOf ;
                                    owl:allValuesFrom lis:PhysicalObjectThatHasCasing
                                  ]
```

This implementation of the approach forces a proliferation of new extensions of existing terms in existing ontologies.

Alternate implementations that in one way or another specifies that the property in question is not directly related to the subject, but the subject is a proxy for one of the parts of the subject still requires us to specify what part is being proxied, and we've rounded all the way back to having to define the part of relation, but we've had to define a new type of attribute relation and we have to do more work each and every time we want to use the attribute.

In summary, the hasPart approach enables re-use of larger parts of more existing reference data collections but necessitates a thorough and consistent plan and approach when deciding how to do type modelling.

The Casing_Length approach minimizes the amount of planning and coordination needed before anyone can start producing types, but at the cost re-use of established reference data collections and reduced re-usability of the newly produced types.

