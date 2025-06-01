# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This project is a visualizer for data from Vermont's Act 250 database. When completed, it will scrape data from https://anrweb.vt.gov/ANR/Act250/default.aspx and, for each Act 250 permit request, return an object that we will use within our visualization.

Users will then be able to manipulate data visualizations that pull from this data in a web browser.

## Project Purpose

This project aims to provide data visualizations on Vermont's Act 250 data with the intent of allowing users to better understand the relationships between specific types of Act 250 reviews and their acceptance/denial rates, the reasons why applications might be denied, and, eventually, this project aims to provide an interactive map of Act 250 data.

## Development Setup

The frontend of this project will be a React/TailwindCSS/TypeScript project.

The backend should use Convex (https://www.convex.dev/)

## Project Phases

Phase 1: Using Puppeteer, set up scraping infrastructure and initial data extraction
- The database is interacted with using a form which allows users to define filters for the data.
- Once filters are selected and the query is submitted, the data returned is limited to 500 results (it's not paginated, it's simply capped at 500 results).
- Each returned result can then be selected and viewed in detail. These detailed views very often--but not always--contain attachment files. We will need to access these.

Phase 2: Implement document parsing and criterion 
- All document attachments are .pdf files.
- The project should only scrape data related to the following Application Types: Major, Major (Minor), Minor
- The project should ignore any projects with the Application Type of either Jurisdictional Opinion or Administrative Amendment. This project is not concerned with that data.
- We are specifically concerned with the data found in a file that has a name similar to "Findings of Fact", "Findings of Fact COS", Findings of Fact COS Exhibit List", "Findings_of_Fact_COS_Exhibit_List", etc. This file's type is "District Commission Documents", and its sub-type is "Land Use Permit & Findings". If a file like this exists, we want to extract the Criterion data from the "Summary Conclusion of Law" paragraph and record this. This data is very valuable for us.

Phase 3: Build data processing pipeline and validation
- For each individual permit, we want to extract data in the following schema whenever it is present:
permit: {
  projectNumber: string, // Should match the "Project Number" field in the "Project Details" table.
  projectName: string, // Should match the "Project Name" field in the "Project Details" table.
  projectAddress: string, // Should match the "Project Address" field in the "Project Details" table. 
  applicationType: 'Major' | 'Major (Minor)' | 'Minor', // Should match the "Application Type" field in the "Project Details" table.
  projectType: string, // Should match the "Project Number" field in the "Project Type" table.
  latitude: string | number, // Should match the "Latitude" field in the "Project Details" table.
  longitude: string | number, // Should match the "Longitude" field in the "Project Details" table.
  dateReceived: Date, // Should match the "Date Application Received" field in the "Project Status" table.
  dateComplete: Date, // Should match the "Date Deemed Complete" field in the "Project Status" table.
  dateDecisionIssued: Date, // Should match the "Date Decision Issued" field in the "Project Status" table.
  currentStatus: 'Abandoned' | 'Cleanup' | 'Denied' | 'Dismissed' | 'EB Appeal' | 'Expired' | 'Findings' | 'Inactivated' | 'JO issued' | 'Pending (Awaiting Information)' | 'Pending (Comment Period') | 'Pending (Decision)' | 'Pending (Hearing)' | 'Pending (in Review)' | 'Pending (Major/Minor Determination)' | 'Permit' | 'Received' | 'Revoked' | 'Submitted' | 'Withdrawn'
  projectUrl: string // Should match the URL for the details page of the project 
}

Phase 4: Develop Convex API/schemas
- TBD

Phase 5: Create React frontend with basic visualizations
- TBD

Phase 6: Add advanced visualizations, including a map view
- This map view should overlay projects on their geographic location with a pin that shows a details summery and links to the projectUrl for that project.
Phase 7: Testing, optimization, and deployment

## Current Phase
- This project is currently in Phase 1 and has not completed any phases yet.

## Instructions for Claude
- Claude should commit any changes to 