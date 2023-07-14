## Running the Cylera Task

This toolkit gets data from Cylera.

**Run the Cylera Task**

To run this task, use the following Cylera information:

1. Cylera instance hostname
2. Cylera api user email
3. Cylera api user password

## Command Line

**Note:** For instructions on running tasks, see the main Toolkit page.

For this task, you can leave off the Kenna API Key and Kenna Connector ID, so the task creates a .JSON file in the default or specified output directory. You can then review the file, before you upload it directly to the Cisco.

**Use these Recommended Steps:**

1. To ensure you can get data properly from the scanner, run it with Cylera keys only. 
2. Review the data output to ensure you got the expected data.
3. Create the Kenna Data Importer connector in Cisco Vunerability Management  (for example, name it: **Cylera KDI**).
4. From Step 1, run the connector manually with the .JSON file. 
5. To get the connector ID, click the name of the connector. 
6. Run the task with Cylera Keys and Kenna Key/connector ID.

## Complete list of Options:

| Option | Required | Description | default |
| --- | --- | --- | --- |
| cylera_api_host | true | Cylera instance hostname, e.g. partner.us1.cylera.com. Must escape hostname in command line script, e.g. \\"partner.us1.cylera.com\\" | n/a |
| cylera_api_user | true | Cylera API user email | n/a |
| cylera_api_password | true | Cylera API user password | n/a |
| cylera_ip_address | false | Partial or complete IP or subnet | n/a |
| cylera_mac_address | false | Partial or complete MAC address | n/a |
| cylera_first_seen_before | false | Finds devices that were first seen before this epoch timestamp | n/a |
| cylera_first_seen_after | false | Finds devices that were first seen after this epoch timestamp | n/a |
| cylera_last_seen_before | false | Finds devices that were last seen before this epoch timestamp | n/a |
| cylera_last_seen_after | false | Finds devices that were last seen after this epoch timestamp | n/a |
| cylera_vendor | false | Device vendor or manufacturer (e.g. Natus) | n/a |
| cylera_type | false | Device type (e.g. EEG) | n/a |
| cylera_model | false | Device model (e.g. NATUS NeuroWorks XLTECH EEG Unit) | n/a |
| cylera_class | false | Device class (e.g. Medical). One of [Medical, Infrastructure, Misc IoT] | n/a |
| cylera_confidence | false | Confidence in vulnerability detection. One of [LOW, MEDIUM, HIGH] | n/a |
| cylera_detected_after | false | Epoch timestamp after which a vulnerability was detected | n/a |
| cylera_name | false | Name of the vulnerability (complete or partial) | n/a |
| cylera_severity | false | Vulnerability severity. One of [LOW, MEDIUM, HIGH, CRITICAL] | n/a |
| cylera_status | false | Vulnerability status. One of [OPEN, IN_PROGRESS, RESOLVED, SUPPRESSED] | n/a |
| cylera_page | false | Controls which page of results to return | 0 |
| cylera_page_size | false | Controls number of results in each response. Max 100. | 100 |
| kenna_api_key | false | Kenna API Key for use with connector option | n/a |
| kenna_api_host | false | Kenna API Hostname if not US shared | api.kennasecurity.com |
| kenna_connector_id | false | If set, it tries to upload to this connector | n/a |
| output_directory | false | If set, it writes a file after it is complete 
| Path is relative to #{$basedir} | output/cylera_id| 

## Data Mappings

| Kenna Asset | from Cylera Devices | Conditions |
| --- | --- | --- |
| ip_address | device.ip_address | |
| mac_address | device.mac_address | |
| os | device.os | |
| tags | ["Vendor:#{device.vendor}", "Type:#{device.type}", "Model:#{device.model}", "Class:#{device.class}"] | if proper value exists |

| Kenna Vulnerability | from Cylera Vulnerability | Conditions |
| --- | --- | --- |
| scanner_identifier | vulnerability.vulnerability_name | |
| scanner_type | "Cylera" | |
| scanner_score | vulnerability.severity | |
| created_at | vulnerability.first_seen | |
| last_seen_at | vulnerability.last_seen | |
| status | vulnerability.status | |
| vuln_def_name | vulnerability.vulnerability_name | |

| Kenna Definition | from Cylera Vulnerability and Mitigations | Conditions |
| --- | --- | --- |
| scanner_type | "Cylera" | |
| cve_identifiers | vulnerability.vulnerability_name | if vulnerability.vulnerability_name starts with "CVE" |
| name | vulnerability.vulnerability_name | |
| solution | "#{mitigation.mitigations}\nAdditional Info\n#{mitigation.additional_info}\nVendor Response\n#{mitigation.vendor_response}" | if proper value exists |
| descriptions | mitigation.description | if value exists |
