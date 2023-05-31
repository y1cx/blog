---
title: Google Analytics
date: 2022-02-25 10:46:48
categories: Skills
tags: Google Analytics
description: Learning notes for Google Analytics.
---
## Beginners

### Introducing Google Analytics

Google Analytics uses javascript code added to your website pages to collect data and send it to Google Analytics.

These data include the user's activity, language, type of browser, device and operating system, traffic source.

The activity is grouped into session, it begins when a user navigates to a page that includes the tacking code and ends after 30 minutes of inactivity. If the user returns to a page after a session ends, a new session will begin.

#### Setup

Organization -> Multiple accounts -> Multiple Property (Tracking ID) -> Multiple View (Filter, Goal).

View Settings:
- Manage users: Add or remove user access
- Edit: Change configuration settings  
- Collaborate: Share parts of data or settings
- Read and analyze: Restricts changes to the settings or adding new users.

Add Filter, e.g., to exclude internal traffic.

### GA Interface

- Customization
- Realtime
- Audience
- Acquisition
- Behavior
- Conversion

### Basic Reports

- Audience
- Acquisition: traffic sources 
  
  Source: search engine, referring site, newsletters, direct (URL)

  Medium: Organic Search (unpaid), CPC (Google Ads paid search), Referral, Email, None (URL)

  Keyword: SSL search

  Campaign: name of the referring Google Ads campaign or a custom campaign

  Content: a specific link or content item in a custom campaign

  <span style="color:orange">Traffic Source dimensions GA automatically capture for each user who comes to your site: Source, Medium, Campaign name.</span>

- Behavior

### Basic Campaign and Conversion Tracking

- Measure Custom Campaigns

Tags: Medium Source Campaign Content Term

Standard Google Analytics campaign parameters: utm_source, utm_medium, utm_campaign, utm_term, utm_content

- Tracing campaigns with the URL Builder

https://ga-dev-tools.web.app/campaign-url-builder/

- User Goals to measure business objectives

Business Goals: conversion

GA Goals: conversion-related metrics, conversion rate -> Goal Funnel

<span style="color:orange">When creating a Goal in Google Analytics, Goal Name, Type and Slot ID are required.</span>

- Measure Google Ads campaigns

<span style="color:orange">Google Ads lets users advertise on properties: Google Search, Google Display Network.</span>

## Advanced

### Data Collection and Processing

1. New vs. Returning Users

cid (a randomly-assigned unique identifier) to differentiate users, depends on cookies (browser, device)

<span style="color:orange">If default Google Analytics tracking code is installed on pages with different domains, Analytics will count these users and sessions separately.</span>

2. Sessions

Pageview hit

Event hit: Action Category Label Value

Transaction hit

Social hit...

GA groups hits to sessions (30 minutes, can be configured: https://support.google.com/analytics/answer/2795871?hl=en)

3. Other data sources

Measurement Protocol

Account Linking

4. Apply configuration rules

- Data filters: true or false

- Goals: Destination/Pageviews, Event, Duration, Pages/Screens per Session -> Goal Completions, Goal Value, Goal Conversion Rate

- Data Grouping

- Custom dimensions

- Custom metrics

- Imported data

5. Store data and generate reports

Dimensions and metrics can have one of three scopes: hit level, session level and user level.

Raw View, Test View, Master View

6. Create a measurement plan

Macro- and micro- conversions

Measurement plan: Business Objective -> Strategy -> Tactic -> KPI

### Setting Up

- Account

Organization or Account (Unique ID) -> Multiple Property (Raw Data, Test, Master)

Multiple properties e.g., Website and Mobile

Cross-domain tracking: site-liking

Roll-up reporting to aggregate properties

- Filters

Pre-defined

Custom: Regular expressions

Orders

- Custom Dimensions

<span style="color:orange">To pair metrics with dimensions, they should have same scope in common.

Custom Dimensions can be used as primary dimensions in Custom Reports, or as secondary dimensions in Standard reports.</span>

- Custom Metrics

<span style="color:orange">Scopes Custom metrics can have: Hit, Product.</span>

- Event Tracking: Behavior -> Event

<span style="color:orange">Four parameters can be included with an event hit for reporting: Category, Action, Label, Value.</span>

- Other configurations: 
  
  Site search Tracking, Calculated Metrics, Channel Groupings, Content Grouping, Enhanced E-commerce, User ID, Data Import

### Advanced Analysis Tools and Techniques

- Segment data: User, Session

- Analyze data by channel (Multi-Channel Funnels (MCF)), audience or custom reports

<span style="color:orange">4 segments may be applied at once.</span>

<span style="color:orange">Google Ads and Google Marketing Platform campaigns served on the Google Display Network are grouped into Display channel.</span>

<span style="color:orange">App pages report analyzes which webpages get the most traffic and highest engagement.</span>

<span style="color:orange">In a “last-click” attribution model, Google Analytics will attribute all of the conversion credit to Last marketing activity.</span>

<span style="color:orange">Google Analytics would credit a channel that contributed to a conversion prior to the final interaction to Assisted conversion.</span>

<span style="color:orange">Cohort Analysis report groups an audience based on acquisition date and compares behavior metrics over several week.</span>

<span style="color:orange">A filter that filters out all data, or dimensions and metrics of different scopes would prevent data from appearing in a Custom Report.</span>

### Advanced Marketing Tools

- Dynamic Remarketing

 <span style="color:orange">To enable remarketing in Google Analytics, Advertising Reporting Features and Google Ads or Display & Video 360 account linking must first be enabled.</span>

 <span style="color:orange">Which remarketing audiences can be defined in Google Analytics? Users who visited a specific page on a website, or played a video on a website, or speak a particular language.</span>

 <span style="color:orange">Remarketing can show relevant ads on Google Display Network, Mobile apps and Google Search..</span>

 <span style="color:orange">The maximum duration a user can be included in a remarketing audience is 540 days.</span>

 <span style="color:orange">An audience list require 1000 user cookies to be eligible for Google Ads Search Ad remarketing.</span>

 <span style="color:orange">To set up Dynamic Remarketing for a retail vertical, the Google Merchant Center must be linked to Google Ads.</span>

 <span style="color:orange">To set up Dynamic Remarketing, Custom Dimension must first be created in Google Analytics.</span>

### Summary 

Collect Data -> Create Reports -> Analyze Reports -> Test Solutions

## Exam Hints

Attribution modeling  is the set of rules that determines how sales and conversions get credited based on touch-points in the conversion path.

Within 35 days can a deleted view be restored.

Site Search  is required to track customer search terms on a website.

Ad type cannot be used to create a Custom Segment.

In Multi-Channel Funnel Reports, default conversions are credited through last campaign, search, or ad.

All Pages report shows which web pages get the most traffic and highest engagement.

View filters may be applied retroactively to any data that has been processed. (False)

Custom Dimensions cannot be shared in the Solutions Gallery.

To send data to Google Analytics from a web-connected device like a point-of-sale system, The Measurement Protocol must be used.

Email campaigns require manual tags on destination URLs for tracking.

When linking a Google Ads account to Google Analytics, it is not possible to adjust keyword bids in Google Ads from Google Analytics.

Goals that are available in Google Analytics: Destination, Event, Duration, Pages/Screens per Session.

Sharing a Custom Report will share the report configuration and data included in the report. (FALSE)

