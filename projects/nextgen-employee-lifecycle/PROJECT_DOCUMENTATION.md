# NextGen Employee Lifecycle
## Employee Onboarding Automation — Project Documentation

**Platform:** ServiceNow  
**Application:** NextGen Employee Lifecycle  
**Organization:** NextGen Technologies (fictional/demo organization)  
**Project scope:** Employee onboarding automation  
**Status:** Core workflow implemented and tested in a Personal Developer Instance (PDI)

---

## 1. Overview

This project provides a centralized Service Catalog item for submitting employee onboarding requests. The request collects employee and job-related information and is routed to the employee's manager for approval.

When approved, the flow updates the Requested Item approval status and creates three department-specific catalog tasks for IT, HR, and Facilities. When rejected, the flow updates the Requested Item approval status to Rejected and does not create those department tasks.

## 2. Objectives

- Collect employee onboarding information through a Service Catalog item.
- Display relevant form fields conditionally.
- Validate the joining date before submission.
- Obtain manager approval.
- Create department tasks when the request is approved.
- Prevent department task creation when the request is rejected.

## 3. Service Catalog item

**Catalog item:** New Employee Onboarding

The item contains 11 variables:

1. Employee Name
2. Personal Email
3. Start Date
4. Department
5. Job Title
6. Manager
7. Laptop Type
8. Needs Software Access
9. Software List
10. Work Location
11. Needs Desk and ID Card

## 4. Catalog UI Policies

Two Catalog UI Policies control conditional visibility:

- **Software access:** Show the Software List variable when Needs Software Access is selected; hide it otherwise.
- **Office requirements:** Show the Needs Desk and ID Card variable when Work Location is Office; hide it otherwise.

## 5. Catalog Client Script

**Name:** Validate Start Date - 5 Days  
**Type:** onSubmit

The script checks the selected Start Date before submission. It displays an error and blocks submission when the date is fewer than five days from the current date. A date at least five days away is accepted.

## 6. Flow Designer

**Flow:** NextGen Employee Onboarding Flow  
**Trigger:** Service Catalog — Catalog Item Requested  
**Execution:** Background

The flow retrieves catalog variables, requests manager approval, evaluates the approval result, and follows one of two paths.

### Approved path

1. Update the Requested Item Approval field to Approved.
2. Create an IT catalog task.
3. Create an HR catalog task.
4. Create a Facilities catalog task.

### Rejected path

1. Update the Requested Item Approval field to Rejected.
2. Do not create the IT, HR, or Facilities tasks.

The three Create Catalog Task actions are configured sequentially in the flow.

## 7. Department tasks

| Department | Task short description | Assignment group |
|---|---|---|
| IT | Prepare IT equipment and software access for new employee | ITSM Engineering |
| HR | Complete HR onboarding documentation for new employee | Not configured |
| Facilities | Prepare workspace and ID card for new employee | Not configured |

The task descriptions ask the relevant team to prepare equipment/access, complete HR joining formalities, and prepare the workspace/ID card.

## 8. Testing evidence

The following end-to-end scenarios were tested in the PDI:

### Approved request

- Submitted a New Employee Onboarding request.
- Confirmed the Service Catalog trigger started the flow automatically.
- Approved the manager approval.
- Confirmed the flow execution completed.
- Confirmed three catalog tasks were created: IT, HR, and Facilities.

### Rejected request

- Submitted a separate New Employee Onboarding request.
- Rejected the manager approval.
- Confirmed the flow execution completed.
- Confirmed the Requested Item Approval field showed Rejected.
- Confirmed no IT, HR, or Facilities catalog tasks were created.

### Start-date validation

- Tested a date fewer than five days from today and confirmed submission was blocked.
- Tested a date at least five days from today and confirmed it was accepted.

## 9. Outcome

The tested onboarding workflow automates the manager approval decision and creates department-specific fulfillment tasks only when the request is approved.

## 10. Scope boundaries and future enhancements

**Implemented:** Employee onboarding request, conditional variable visibility, joining-date validation, manager approval, approval/rejection handling, and automatic creation of three department catalog tasks.

**Not implemented in this version:** Employee offboarding, email notifications, dedicated HR and Facilities assignment groups, task completion tracking, role-based access controls, dashboards/reports, external integrations, and production deployment.

These may be considered as future enhancements; they should not be represented as completed features.

---

**Note:** This is a demonstration project built with dummy data in a ServiceNow Personal Developer Instance. It is not a production deployment.
