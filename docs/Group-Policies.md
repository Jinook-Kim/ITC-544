# Group Policies

## Overview

Summarize how Group Policy Objects (GPOs) are used to manage users and devices.

## Policy Summary

| GPO Name | Linked OU | Affected Users/Devices | Purpose |
| -------- | --------- | ---------------------- | ------- |
|          |           |                        |         |

## Policies Applied

List key GPOs by user or device group.

| User/Device Group | Applied Policies | Description |
| ----------------- | ---------------- | ----------- |
|                   |                  |             |

## Procedures

### Creating a New GPO

1. Open Group Policy Management Console (GPMC)
2. Right-click the target OU → _Create a GPO in this domain_
3. Name the GPO appropriately
4. Edit the GPO and configure settings
5. Link it to the appropriate OU
6. Run `gpupdate /force` to apply changes

### Modifying an Existing GPO

1. Open GPMC
2. Edit the desired GPO
3. Document all changes below:

| Date | Change Description | Edited By |
| ---- | ------------------ | --------- |
|      |                    |           |

# 1. Policies Applied to User Groups
## Password & Security Policy

Minimum password length: 12 characters

Enforce password complexity (uppercase, lowercase, number, symbol)

Account lockout after 5 failed login attempts

Password history enforced to prevent reuse

Applies to all user groups

## Software Deployment (SD) Policy
Content_Dept

Allowed to install Microsoft applications only

All other software installations are blocked

All Other User Groups

No installation rights

Must use IT-approved, centrally deployed software only

IT_Admins / IT_Dept / IT_Executives

Full install rights only on:

Admin workstations

Servers

Installation on standard devices limited to support tasks only

Enforcement Tools

AppLocker / Software Restriction Policies

Removal of all local admin rights

Installation attempts logged for compliance monitoring

## Access Control Policy

Access based strictly on group membership

Servers Group

Only server admins and IT_Admins have access

Admin_Workstations

Admin rights only for IT staff

Standard Users

Restricted from:

System directories

Admin consoles

Sensitive resources

Department Groups

(Sales, Content, Graphics, HR, BI, etc.)

Access only to applications needed for their role

## Administrative Rights Assignment
Full Administrative Rights

IT Managers

Network Admins

Members of:

IT_Admins

IT_Dept

IT_Executives

Servers

Delegated Rights

Executives (CTO, Directors)

Limited admin access for business-critical tools only

Standard Rights

All other groups

Customer_Service

Sales

HR

Content

Graphics

Etc.

No local admin privileges

# 2. Policies Applied to Device Groups
## Admin_Workstations

Full administrative permissions

Access to diagnostic and configuration tools

Can install and manage software

Used by IT staff only

## Workstations (Standard User Devices)

No admin rights for end users

Company-standard desktop configuration

Mandatory IT-approved software only

Restricted access to OS settings and Control Panel

## Servers Group

Strictly limited administrative access

Server hardening baseline applied

Installation limited to approved server software

Logging and monitoring enforced

## Device Configuration Rules

Corporate theme and standardized settings

Locked-down system configurations

Restricted removable-media access for BI, HR, and IT

Automatic updates enforced

Microsoft Defender Antivirus and endpoint protection required
