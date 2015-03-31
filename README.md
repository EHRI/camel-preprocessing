# Pipeline for preprocessing EAD files in the EHRI project

This repository contains a Blueprint file specifying Camel routes and endpoints to get EAD files exported from [REPOX][repox], process them directly or by calling XSLT files from the [EHRI EAD preprocessing tools][ehri-ead-preprocessing] and finally importing them into the [graph database][ehri-rest] that powers the [EHRI portal][portal].

The example routes are setup with a specific repository loaded in REPOX (the Wiener Library) and corresponding paths in mind. These paths are:

- REPOX's export path
- paths to folders to store intermediate files and final files
- the location of the `git clone` of the [preprocessing tools][ehri-ead-preprocessing]

REPOX is normally deployed in Tomcat, which means that the directories that REPOX manages get the file permissions that Tomcat has.

The routes are deployed to [Apache ServiceMix][servicemix] by moving or copying the Blueprints XML file in ServiceMix's `deploy` directory. The `log:display` or `log:tail` ServiceMix commands will display respectively tail the log and if all goes well, show messages of progress - or errors if not everything goes well.

[repox]: https://github.com/europeana/REPOX
[ehri-ead-preprocessing]: https://github.com/EHRI/ehri-ead-preprocessing
[ehri-rest]: https://github.com/EHRI/ehri-rest
[portal]: https://portal.ehri-project.eu
[servicemix]: http://servicemix.apache.org
