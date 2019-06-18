title: Kimball Dimensional Modeling Techniques Overview
date: 2019-06-17 20:28:10
tags:
- Datawarehouse
- Kimball
---

# Fundamental Concepts

## Gather Business Requirements and Data Realities

samples in the book

Chapter 1 DW/BI and Dimensional Modeling Primer , p 5
Chapter 3 Retail Sales , p 70
Chapter 11 Telecommunications , p 297
Chapter 17 Lifecycle Overview , p 412
Chapter 18 Dimensional Modeling Process and Tasks , p 431
Chapter 19 ETL Subsystems and Techniques ,p 444

## Collaborative Dimensional Modeling Workshops

Dimension models should be designed by folks who fully understand the business and their needs.

## Four-Step Dimensional Design process

* Select the business Process
* Declare the Grain
* Identify the Dimensions
* Identify the facts

## Business Processes

Operational Activities

## Grain

The grain establishes exactly what a single fact table row represents.

## Dimensions for Descriptive Context

## Facts for Measurements

## Star Schemas and OLAP Cubes


## Graceful Extensions to Dimensional Models

* Add column to Fact table to describe FACT
* Add column to Fact table to contain foreign key to new dimension table
* Add column to Dimension table to add Attributes


# Basic Fact Table Techniques

## Fact Table Structure
A fact table contains the numeric measure produced by an operational measurement event in the real world.

## Additive, Semi-Additive, Non-Additive Facts

Balance amounts are common semi-additive facts because they are additive across all dimensions except time.
Some measures are completely non-additive, such as ratios.

## Nulls in Fact Tables
P79

# Basic Dimension Table Techniques


# Integration via Conformed Dimensions

# Dealing with Slowly Changing Dimension Attributes

# Dealing with Dimension Hierarchies

## Fixed Depth Positional Hierarchies

## Slightly Ragged/Variable Depth Hierarchies

## Ragged/Variable Depth Hierarchies with Hierarchy Bridge Tables

## Ragged/Variable Depth Hierarchies with Pathstring Attributes

# Advanced Fact Table Techniques


# Advanced Dimension Techniques


# Special Purpose Schemas
