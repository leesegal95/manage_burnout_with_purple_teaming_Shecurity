# Alert-Host-Entry-File-Updated

## Goal

> The goal is the intended purpose of the alert. It is a simple, plaintext description of the type of behavior you're attempting to detect in your ADS.

The goal of this Alert is detect if a users host file entry was updated on their box. 

## Technical Context / Abstract

> provides detailed information and background needed for a responder to understand all components of the alert.

> Ideally, the responder, regardless of experience, should receive enough context in this section to make an informed judgement call on whether the alert is a false or true positive.

Host entry file: responsible for DNS resolution, correlates the URL to IP address. If this file id updated it can lead to a malicious IP address correlating to a site the user believes is trusted.

The file can be found on an ubuntu box in this location: etc/hosts

Using osquery (agent on box which monitors the our file management. To see what osquery monitor go to this location: etc/osquery/osquery.conf) we can see that this file has been updated. We found the logs in this location: var/log/osqeury/osquery.results.log. When looking at the logs you can see that the file etc/hosts had been updated. Using those logs we created an alert to detect when this log has been updated. 

Extremely rare instances would this file be manually altered on a single box.

## Blind Spots and Assumptions

> No alert is perfect. In this section, give an overview of any assumptions or blind spots the alert might have.

> * What are we assuming will work properly for the alert to fire correctly?
> * When might this alert miss a true positive?

We are assuming that osquery is configured and working as expected on the box. 

## False Positives

> Even the best alerts have the potential to hit on false positives. In this section:

> * Detail known instances when this alert will misfire.
> * Provide guidance on how to tell a false positive from a true positive. 

> The goal is to limit the number of known false positives before an alert is deployed in production. If the number of false positives remain especially high for an alert even after steps were taken to minimize them, it is possible that the alert should be discarded.

There should not be false positives. The hosts file should not be manually updated. 

## Validation

> Every effective alert needs to be validated to ensure that it will trigger on a true positive. You don’t want to deploy a dud alert that simply places a burden on our alerting pipeline without producing results.
 
> In this section, you will detail the scripts, commands, or actions that need to be taken in order to produce a true positive within the environment. 

Use purple teaming exercise or local testing to validate this alert. In our test environment, we went to the host file and updated the entry. 

## Priority

> Describe the severity of the alert (low, medium, high, critical) and any scenarios that might raise/lower the severity. 

HIGH Priority. 

## Response

> This section details the process of triaging and investigating an alert once it fires. It should provide detail into the next steps the responder should take and should link to any relevant incident playbooks.
On-call engineer should contacting the usr who had the public key added and investiagting their box. First by checking the file to see whose public key was added. 

Immediately contact the user of the box and have them investigate their hosts file. See if any changes where made. Assume that, that user has been compromised and take actions to reset passwords and reset computer. Update host file to correct form. 

## Future Improvement Ideas

> Any future improvement ideas for the alert, such as additional automation or improved fidelity.

NA

> Postscript: This document template is heavily inspired by Palantir’s [Alerting and Detection Strategies (ADS) Framework](https://github.com/palantir/alerting-detection-strategy-framework). The structure is nearly identical and while the verbiage is modified, it follows closely to the core ideas presented by Palantir’s Information Security team. We thank them for their contributions to the InfoSec community as a whole and hope to contribute back as they have once we have matured our capabilities. 