# SQL: Employee Database

## Background

It’s a beautiful spring day, and it’s been two weeks since you were hired as a new data engineer at Pewlett Hackard. Your first major task is a research project on employees of the corporation from the 1980s and 1990s. All that remains of the database of employees from that period are six CSV files.

In this assignment, you will design the tables to hold data in the CSVs, import the CSVs into a SQL database, and answer questions about the data. In other words, you will perform **data modeling**, **data engineering**, and **data analysis**.

## Performed in this assignment:

 - Climate Analysis and Exploration
 - Precipitation Analysis
 - Station Analysis
 - Climate App

## Step 1 - Climate Analysis and Exploration

Python and SQLAlchemy was used to do basic climate analysis and data exploration of a climate database. The analysis was completed using SQLAlchemy ORM queries, Pandas, and Matplotlib.

# Precipitation Analysis

Query designed to retrieve the last 12 months of precipitation data.
![2](https://user-images.githubusercontent.com/108673720/193393180-056dc960-7906-4309-9dba-cdc0956bffde.png)

# Station Analysis

Query designed to calculate the total number of stations.
![1](https://user-images.githubusercontent.com/108673720/193393205-3ad2aae7-1197-4826-9abc-dd0270df49c5.png)

- - -

## Step 2 - Climate App

Flask API designed based on the queries developed during the inital step.

# Routes
```
    /api/v1.0/tobs
    
    /api/v1.0/(start)
    
    /api/v1.0/(start)/(end)
```
