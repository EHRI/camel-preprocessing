---
title: Building a pre-processing pipeline for EHRI metadata records
author: Ben Companjen
date: 2015-03-19
---

# Introduction

The [EHRI portal][portal] provides access to collection and item descriptions relating to the Holocaust from archives, libraries and museums from Europe and beyond. These descriptions are either created and edited manually from the portal or provided in digital form and imported into the database that powers the portal.

Collection holding institutes (CHIs) who provide their descriptions to EHRI in digital are requested to do so in the [Encoded Archival Description (EAD)][ead] format following [EHRI's Guidelines for Description][ehri-guidelines], which are based on [ISAD(G)][isadg]. The preferred method of delivery of these files is [OAI-PMH][oai-pmh], a well-known protocol for harvesting metadata over the web. OAI-PMH supports full and incremental collection of metadata records, notifications of deleted records and downloading specified sets of records. If implemented correctly, EHRI could download relevant EADs from CHIs and update the record in the database.

However, this is where theory and practice diverge.

Only a few CHIs were able to offer records via OAI-PMH and none do it in such a way that import is straightforward. Differences between the above ideal situation and reality include:

- there is no set that contains just the records that had been selected for EHRI. This means the records have to be filtered after the harvest, or individual records need to be retrieved.
- the OAI-PMH endpoint behaves in unexpected ways. One endpoint did not stop sending records after the set of records had been downloaded completely, but kept sending "there is more" and started to repeat records.
- the XML does not use XML Namespaces correctly, so that the actual metadata was not separable from the container XML specified by OAI-PMH.
- the EAD has content issues. In one example, descriptions of parts of a documentary unit were in separate records instead of being nested in an EAD hierarchy. In other cases, the EHRI (or ISAD(G)) guidelines were not followed and EHRI had to reorder metadata in the records.
- the importer that loads and inserts or updates the metadata from EAD files cannot handle some valid EAD elements. The current implementation of the EAD importer cannot import mixed content correctly, so text paragraphs with EAD markup elements do not import as expected.

## About this document

This document outlines the setup and use of open source components as a Pre-processing Pipeline (PP) to automate the ingest, pre-processing and import of EAD files. The next section will discuss the component that manages the ingest from OAI-PMH endpoints and folder. Section 3 will discuss the integration of various pre-processing components into a pipeline. Section 4 discusses the automated import of metadata records. Finally the last section will discuss some outstanding issues and limitations.

# Ingest

The PP starts at the ingest process. This is handled by the [REPOX][repox] software, that can ingest XML metadata from OAI-PMH endpoints, folders on the file system, HTTP endpoints and FTP servers. It can also act as an SRU Update endpoint to get updated records pushed to it. REPOX is currently maintained by [Europeana][europeana], who use it in the infrastructure of [The European Library][tel] to harvest bibliographical metadata from national and research libraries around Europe.

REPOX has a web interface and a REST interface to manage the (minimal) administrative information about data providers and mostly technical information about the data sets they provide. REPOX can ingest records on demand, or following a schedule. Similarly, exporting the records to a folder on the file system can happen on demand or following a schedule.

# Pre-processing

EHRI has produced various pre-processing tools to normalise or even correct ingested records. These tools should be used before importing the data into the database using the importer tools, because the importer tools have requirements that not all records adhere to.

Metadata from specific CHIs need more preprocessing than from others. Some of these tools must be called in a certain order when used. Some tools are XSLT stylesheets, others are Java processors packaged as a JAR file.

## Implementation

Camel in ServiceMix 5.4.0

basic installation, install OSGi features `camel-saxon` and `camel-http`.

## Pipelined tools

Filter, unwrap, convert `<emph>` within `<p>`, convert `<extref>` to Markdown, remove `<emph>` in `<persname>`, rename and sort files into specific folders.

# Import

The EHRI portal backend allows calling an HTTP endpoint to start an import process.

single file endpoint

throttle

# Test results

It works

Creates import event for each file which is suboptimal.

Reindexing still decoupled.



[portal]: https://portal.ehri-project.eu/
[ead]: http://www.loc.gov/ead/
[ehri-guidelines]: https://ehri.basecamphq.com/projects/7720758/file/191762349/DL15%205%20Exhibit%203%20D%2017%203%20Guidelines%20update%20July%202014.pdf
[isadg]: http://www.icacds.org.uk/eng/ISAD%28G%29.pdf
[oai-pmh]: http://www.openarchives.org/OAI/2.0/openarchivesprotocol.htm
[repox]: https://github.com/europeana/REPOX
[europeana]: http://www.europeana.eu/
[tel]: http://www.theeuropeanlibrary.org/

