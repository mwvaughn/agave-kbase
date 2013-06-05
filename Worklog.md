## Craft application bundle for all_entities_Genome and eventually other apps

```
tree kbase-20130604
stampede
	json
		kbase_all_entities_Genome.json
	kb.tgz
	templates
		all_entities_Genome.template
		all_entities_Genome.test.sh
		generic-cdm-query.sge
		test.sh
	test_data
		*
```

## Push into iPlant Data Store

Can also irsync for finer-grained control...

```
module load irods python
iput -frPVT kbase-20130604 /iplant/home/vaughn/applications/
```

## Register app with apps-v1/apps service (one must have app publication rights for now)

```
curl -X POST -sku "vaughn" -F "fileToUpload=@kbase-20130604/stampede/json/kbase_all_entities_Genome.json" https://foundation.iplantc.org/apps-v1/apps | python -mjson.tool
```

## Result

Note 3-stanza message with new app description in result payload. The entity result.id "kbase_all_entities_Genome-20130604" is how we will invoke the app as a service

```
{
    "message": "", 
    "result": {
        "available": true, 
        "checkpointable": false, 
        "deploymentPath": "/vaughn/applications/kbase-20130604/stampede", 
        "executionHost": "stampede.tacc.xsede.org", 
        "executionType": "HPC", 
        "helpURI": "http://kbase.science.energy.gov/", 
        "id": "kbase_all_entities_Genome-20130604", 
        "inputs": [], 
        "label": "", 
        "longDescription": "Retrieve entities from Kbase Genome", 
        "modules": [
            "purge", 
            "load TACC", 
            "load irods", 
            "swap intel gcc"
        ], 
        "name": "kbase_all_entities_Genome", 
        "ontolog": [
            "http://sswapmeet.sswap.info/agave/apps/Application"
        ], 
        "outputs": [
            {
                "defaultValue": "stdout_all_entities_Genome", 
                "details": {
                    "description": "", 
                    "label": "Output of all_entities_Genome"
                }, 
                "id": "stdout_all_entities_Genome", 
                "semantics": {
                    "fileTypes": [
                        "tab-0"
                    ], 
                    "maxCardinality": -1, 
                    "minCardinality": 1, 
                    "ontology": [
                        "http://sswapmeet.sswap.info/mime/text/Tab-separated-values"
                    ]
                }, 
                "value": {
                    "default": "stdout_all_entities_Genome", 
                    "validator": ""
                }
            }
        ], 
        "parallelism": "SERIAL", 
        "parameters": [
            {
                "defaultValue": "false", 
                "details": {
                    "description": "", 
                    "label": "Return all available fields"
                }, 
                "id": "a", 
                "semantics": {
                    "ontology": [
                        "xs:boolean"
                    ]
                }, 
                "value": {
                    "default": "false", 
                    "required": false, 
                    "type": "bool", 
                    "validator": "", 
                    "visible": true
                }
            }, 
            {
                "defaultValue": "false", 
                "details": {
                    "description": "", 
                    "label": "List the available fields"
                }, 
                "id": "show-fields", 
                "semantics": {
                    "ontology": [
                        "xs:boolean"
                    ]
                }, 
                "value": {
                    "default": "false", 
                    "required": false, 
                    "type": "bool", 
                    "validator": "", 
                    "visible": true
                }
            }, 
            {
                "defaultValue": "", 
                "details": {
                    "description": null, 
                    "label": "Comma-separated set of fields to return"
                }, 
                "id": "fields", 
                "semantics": {
                    "ontology": [
                        "xs:string"
                    ]
                }, 
                "value": {
                    "default": "", 
                    "required": true, 
                    "type": "string", 
                    "validator": ",|pegs|rnas|scientific_name|complete|prokaryotic|dna_size|contigs|domain|gc_content|phenotype|md5|source_id", 
                    "visible": true
                }
            }
        ], 
        "public": false, 
        "revision": 2, 
        "shortDescription": "all_entities_Genome", 
        "tags": [
            "kbase", 
            "cdm"
        ], 
        "templatePath": "templates/all_entities_Genome.template", 
        "testPath": "templates/all_entities_Genome.test.sh", 
        "version": "20130604"
    }, 
    "status": "success"
}
```

## Retrive new application

```
curl -X GET -sku "vaughn" https://foundation.iplantc.org/apps-v1/apps/kbase_all_entities_Genome-20130604 | python -mjson.tool
```

(Should just work)

## Test invoke fields, a, and show-fields use cases

```
curl -X POST -sku "vaughn" -d "softwareName=kbase_all_entities_Genome-20130604&jobName=all_entities_Genome_fields&archive=1&requestedTime=00:20:00&fields=contigs,scientific_name" https://foundation.iplantc.org/apps-v1/job | python -mjson.tool
```
