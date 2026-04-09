# LS 563: Linked Data Project, Fall 2026
Hi there! You're looking at the README for Whitney P.'s linked data project for LS 563: Linked Data at the University of Alabama's MLIS Program, Spring 2026.
## About This Dataset
This dataset is comprised of 50 items from my personal library with the following characteristics:
+ Written by a female-presenting author
+ Book has a mystery or thriller genre
+ No two items are written by the same author
+ Book was published in the 21st century

Additionally, the selection represents a variety of authors and publishing houses that represent multiple countries, and the list is comprised of both well-known authors (like Gillian Flynn) and more obscure, indie-published authors (like Stephanie Rose).

My intentions with this project and dataset were to accomplish the goals of this particular class while also addressing some of my other interests, like diversity in cataloging and collection development, critical cataloging, and digital collections. I crafted this project with the mindset of a librarian making a digital collection or creating a display on women-penned mysteries and thrillers with a mix of well-known and more obscure titles published in the year 2000 and later.

Within the dataset, you'll find hardcover, paperback, and ebook items. All of these items are books, so the entity type is always Book; the dataset should be reviewed with that in mind. Each property-value pair is derived directly from the item itself; each piece of information is on the physical copy I own (or digital file, for the ebooks that are included). This matches the practice of original and descriptive cataloging for library catalogers. If data was not found in the book itself, I did not list it; the most notable instances of this are not listing price or number of pages for ebooks and not listing LCSH subject headings for books that did not include them in the title page verso.
## Ontologies and Controlled Vocabularies
The following controlled vocabularies and ontologies can be found in the project and dataset, with brief descriptions as to why they were used:
### BIBFRAME
BIBFRAME was created as an answer to MARC21 for a Linked Data-centric world, so it was a natural inclusion for this project and course. I was less versed in BIBFRAME personally at the beginning of this project, and also sought to strengthen my knowledge.
### Dublin Core
Because BIBFRAME does not have specific properties for all data types, Dublin Core is included because it naturally aligns with the property-value pairs of my dataset and works well with RDF.
### Library of Congress Subject Headings (LCSH)
LCSH provides URIs for all of its subject headings, so including this vocabulary for my project, specifically for the LCSH subject heading source field, is a no-brainer.
### VIAF
VIAF, the Virtual International Authority File, is used for name authority files and records and is a common source for URIs for people and organizations. It is used here for both Publisher and Author source fields.
### Wikidata
Especially for the more obscure titles and authors included in my dataset, VIAF might not have entries for everyone and in fact did not as I reconciled my data. Wikidata was reconciled against to fill in the gaps from the publisher side of things where VIAF was unable to do so. In a couple of instances, I had to personally create the Wikidata entries for two indie publishing houses.
### RDF
RDF is, of course, the standard base model for Linked Data, so it will provide the backbone of the dataset and fill in the blanks where needed, including with blank nodes.
## Linking Strategy
The following source fields are included in the dataset, with their RDF property, related vocabulary/namespace, and their value type. More information on this can be found in the mapping table CSV.
| Source Field | RDF Property | Vocab/Namespace | Value Type |
|-------------:|-------------:|----------------:|------------|
|    Book Title|      bf:title| BIBFRAME | Literal |
|        Author|      bf:agent| BIBFRAME | URI |
|     Pub. Year|       dc:date| Dublin Core | Literal |
|     Publisher|  dc:publisher| Dublin Core | URI |
|          LCSH|    bf:subject| BIBFRAME | URI |
|  No. of Pages|     bf:extent| BIBFRAME | Literal |
|        Format|     dc:format| Dublin Core | Literal |
|          ISBN|     rdf:value| RDF | Literal |
|          LCCN|     rdf:value| RDF | Literal |
|  Price in USD|bf:acquisitionTerms| BIBFRAME | Literal |

Except for the case of Book Title, because it was the main source field all other fields were connecting to, any field that has a specific name or presence that can be linked to other items was given a URI (Publisher, Author, and LCSH Subject Heading). For simplicity, only the first LCSH subject heading for any book was included if more than one were present. All other items received a Literal for their value type. In terms of which vocabularies were used for the fields with URIs, VIAF is commonly used for both publishers and authors in cataloging for authority records, so it was used here for this dataset of books. Where VIAF did not have an entry, Wikidata was instead used to fill in the blanks. LCSH is the obvious direct line for URIs for the LCSH field.

Most literal types in the dataset are directly correlated to their field type (Pub. Year is xsd:gYear, Price in USD is xsd:string) but the one outlier is Number of Pages. Because I normalized the field to include a "p." at the end of the number, as it would be recorded in a MARC entry in a library catalog, the value type for Number of Pages is a literal of xsd:string instead of xsd:integer.

The source fields Book Title, LCSH, and Author are directly tied to Book as a Work; Publisher, Price in USD, Pub. Year, Number of Pages, Format, ISBN, and LCCN are directly tied to Book as an Instance. Work and Instance are connected via the bf:hasInstance property.

A note: For ISBN and LCCN, they were set up as Blank Nodes referring to the bf:identifiedBy property, and then directed from the identifier to the literal via rdf:value. This can be seen represented in the conceptual model.
## Sample Triples
A sample of the conceptual model represented by an item in my dataset can be seen below the general conceptual model in that PDF file. Below are some samples of RDF Triples from my dataset:

(Table form)
| Subject | Predicate | Object |
|--------:|----------:|--------|
| Lucy Foley | is bf:agent of | The Guest List |
| The Guest List | has bf:extent | $27.99 |
| William Morrow | is bf:publisher of | The Guest List |
| Married people--Fiction | is bf:subject of | The Couple Next Door |

(Informal, sentence form)
```
<Lucy Foley><wrote><The Guest List>.
<The Guest List><has the price of><$27.99>.
<William Morrow><published><The Guest List>.
<Married people--Fiction><is the LCSH subject of><The Couple Next Door>.
```

(One example with URIs/literals)
```
<Lucy Foley>       <http://viaf.org/viaf/67749665>
<wrote>            <http://id.loc.gov/ontologies/bibframe/Agent>
<The Guest List>   <"The Guest List"^^@en>
OR
<http://viaf.org/viaf/67749665> <http://id.loc.gov/ontologies/bibframe/Agent> <"The Guest List"^^@en> .
```
