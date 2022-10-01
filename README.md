# SQL: Employee Database

## Background

It’s a beautiful spring day, and it’s been two weeks since you were hired as a new data engineer at Pewlett Hackard. Your first major task is a research project on employees of the corporation from the 1980s and 1990s. All that remains of the database of employees from that period are six CSV files.

In this assignment, you will design the tables to hold data in the CSVs, import the CSVs into a SQL database, and answer questions about the data. In other words, you will perform **data modeling**, **data engineering**, and **data analysis**.

## Performed in this assignment:

 - Climate Analysis and Exploration
 - Precipitation Analysis
 - Station Analysis
 - Climate App

# Step 1 - Climate Analysis and Exploration

Python and SQLAlchemy was used to do basic climate analysis and data exploration of a climate database. The analysis was completed using SQLAlchemy ORM queries, Pandas, and Matplotlib.

## Precipitation Analysis

Query designed to retrieve the last 12 months of precipitation data.

![2](https://user-images.githubusercontent.com/108673720/193393180-056dc960-7906-4309-9dba-cdc0956bffde.png)

## Station Analysis

Query designed to calculate the total number of stations.

![1](https://user-images.githubusercontent.com/108673720/193393205-3ad2aae7-1197-4826-9abc-dd0270df49c5.png)

- - -

# Step 2 - Climate App

Flask API designed based on the queries developed during the inital step.

## Routes
```
    /api/v1.0/tobs
    
    /api/v1.0/(start)
    
    /api/v1.0/(start)/(end)
```

## Script
```
import numpy as np
import pandas as pd
import datetime as dt
import sqlalchemy

from sqlalchemy.ext.automap import automap_base
from sqlalchemy.orm import Session
from sqlalchemy import create_engine, func

from flask import Flask, jsonify


#################################################
# Database Setup
#################################################

engine = create_engine("sqlite:///Resources/hawaii.sqlite")

# reflect an existing database into a new model
Base = automap_base()
# reflect the tables
Base.prepare(engine, reflect=True)

# Save reference to the table
Measurements = Base.classes.measurement
Stations = Base.classes.station

#################################################
# Flask Setup
#################################################

app = Flask(__name__)

#################################################
# Flask Routes
#################################################

#Use Flask to create your routes, as follows:
#        * `/`
#        * Homepage.
#        * List all available routes.

@app.route("/")
def Homepage():
    """List all available routes."""
    return (
        f"Available Routes:<br/>"
        f"/api/v1.0/precipitation<br/>"
        f"/api/v1.0/stations<br/>"
        f"/api/v1.0/tobs<br/>"
        f"""/api/v1.0/(start)<br/>"""
        f"/api/v1.0/(start)/(end)"
    )

# Precipitation Analysis
# `/api/v1.0/precipitation`
#    * Convert the query results to a dictionary using `date` as the key and `prcp` as the value.
#    * Return the JSON representation of your dictionary.

@app.route("/api/v1.0/precipitation")
def precipitation():
    
    session = Session(engine)

    query = [Measurements.date,Measurements.prcp]
    
    weather_data = session.query(*query).all()
    
    all_weather_dict = []
    for date, prcp in weather_data:
        weather_dict = {}
        weather_dict[date] = prcp
        all_weather_dict.append(weather_dict)

    session.close()

    return jsonify(all_weather_dict)

# Station Analysis
# `/api/v1.0/stations`
#    * Return a JSON list of stations from the dataset.

@app.route("/api/v1.0/stations")
def passengers():
    
    session = Session(engine)

    results = session.query(Stations.station).all()

    session.close()

    stations = list(np.ravel(results))

    return jsonify(stations)

# Tobs
#    * `/api/v1.0/tobs`
#    * Query the dates and temperature observations of the most active station for the previous year of data.
#    * Return a JSON list of temperature observations (TOBS) for the previous year.

@app.route("/api/v1.0/tobs")
def tobs():

    session = Session(engine)

    st_date = dt.date(2017,8,18)- dt.timedelta(days=365)
    query4 = [Measurements.date, Measurements.tobs]

    USC00519281_data = session.query(*query4).\
    filter(Measurements.station == 'USC00519281').\
    filter(Measurements.date >= st_date).all()

    USC00519281 = list(np.ravel(USC00519281_data))

    return jsonify(USC00519281)

#################################################
# `/api/v1.0/<start>` and `/api/v1.0/<start>/<end>`
#################################################

#    * `/api/v1.0/<start>`
@app.route("/api/v1.0/<start>")
def start_stats(start):

    session = Session(engine)
    
#    * When given the start only, calculate `TMIN`, `TAVG`, and `TMAX` for all dates greater than or equal to the start date.
    query = [func.min(Measurements.tobs), func.avg(Measurements.tobs), func.max(Measurements.tobs)]

    start_summary = session.query(*query).\
    filter(Measurements.date >= start).all()
    summary = list(np.ravel(start_summary))
    
    return jsonify(summary)

#    * `/api/v1.0/<start>/<end>`    
@app.route("/api/v1.0/<start>/<end>")
def startend_stats(start,end):

    session = Session(engine)

#   * When given the start and the end date, calculate the `TMIN`, `TAVG`, and `TMAX` for dates from the start date through 
#        the end date (inclusive).
    query = [func.min(Measurements.tobs), func.avg(Measurements.tobs), func.max(Measurements.tobs)]

    startend_summary = session.query(*query).\
    filter(Measurements.date >= start).\
    filter(Measurements.date <= end).all()
    summary = list(np.ravel(startend_summary))
    
    return jsonify(summary)

if __name__ == '__main__':
    app.run(debug=True)

## Hints

#* You will need to join the station and measurement tables for some of the queries.
#* Use Flask `jsonify` to convert your API data into a valid JSON response object.
```
