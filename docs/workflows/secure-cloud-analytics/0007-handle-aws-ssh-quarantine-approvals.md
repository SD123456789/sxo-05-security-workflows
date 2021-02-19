---
layout: page
title: Handle AWS SSH Quarantine Approvals
permalink: /workflows/secure-cloud-analytics/0007-handle-aws-ssh-quarantine-approvals
redirect_from:
  - /workflows/sca/0007-handle-aws-ssh-quarantine-approvals
parent: Cisco Secure Cloud Analytics
grand_parent: Workflows
---

# Handle AWS SSH Quarantine Approvals
<div markdown="1">
Workflow #0007
{: .label }
</div>

This workflow is triggered when an Approval Task generated by [Quarantine AWS Instances from Alerts]({{ site.baseurl }}/workflows/secure-cloud-analytics/0006-quarantine-aws-from-alert) is approved, denied, or expires. If approved, SSH quarantine restrictions are removed from the AWS security group.

**Note:** This workflow is designed to respond to approval tasks generated by [this workflow]({{ site.baseurl }}/workflows/secure-cloud-analytics/0006-quarantine-aws-from-alert)!

[<i class="fab fa-github mr-1"></i> GitHub]({{ site.github.repository_url }}/tree/Main/Workflows/0007-SCA-HandleAWSSSHQuarantineApprovals__definition_workflow_01KB7IIMZN9BP0qtnpiqeNnkg14mgEjiILo/){: .btn-cisco-outline }

---

## Requirements
* The following atomic actions must be imported before you can import this workflow:
	* Webex Teams - Post Message to Room ([Github_Target_Atomics]({{ site.baseurl }}/default-repos))
	* Webex Teams - Search for Room ([Github_Target_Atomics]({{ site.baseurl }}/default-repos))
* An Amazon Web Services account with instances monitored by SCA
* (Optional) A Webex Teams access token and room name to post messages to

---

## Workflow Steps
1. (Optional) Add global variables to local variables
1. Extract the AWS instance ID from the Approval Task
1. If a Teams room name was provided, translate it into a room ID
1. Make sure we got an instance ID (if not, post an error to webex)
1. Check the approval result. If the user selected to leave the instance quarantined or the task expired, do nothing. If they want to remove quarantine:
	* Get information about the instance from AWS and extract its security group
	* Restore normal SSH access
	* Send a Webex Teams notification

---

## Configuration
* Set your AWS region in the `AWS Region` local variable
* See [this page]({{ site.baseurl }}/atomics/webex#configuring-our-workflows) for information on configuring the workflow for Webex Teams

---

## Targets
Target Group: `Default TargetGroup`

| Target Name | Type | Details | Account Keys | Notes |
|:------------|:-----|:--------|:-------------|:------|
| Amazon Web Services | AWS Endpoint | _Region:_ `Your Region`<br /> | Your AWS Account Key | |
| Webex Teams | HTTP Endpoint | _Protocol:_ `HTTPS`<br />_Host:_ `webexapis.com`<br />_Path:_ None | None | Not necessary if Webex Teams activities are removed |

---

## Account Keys

| Account Key Name | Type | Details | Notes |
|:-----------------|:-----|:--------|:------|
| Your AWS Account Key | AWS Credentials | _Access Key:_ AWS API Access Key<br />_Secret Key:_ AWS API Secret Key | |