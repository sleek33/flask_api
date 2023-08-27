# flask_api

#i need help, the proprty data has refusd to load onto postman using GET. i have tried all possible means but its not uploading. below is the flask pi code and ps_utility coe nd as well as th json file containig th loctions i wan to lod up ostman. 

from flask import Flask, request, jsonify
from flask_cors import CORS
import ps_util


app = Flask(__name__)
# cors = CORS(app, resources={r"/*": {"origins": "*"}})

@app.route("/get_type_names")
def get_type_names():
    response = jsonify({
        "type": ps_util.get_type_names()
    })
    return response

@app.route("/get_features_with_location_names")
def get_location_names():
    location_names = ps_util.get_features_with_location_names()
    print("Location Names:", location_names)  # Add this line
    response = jsonify({
        "location": location_names
    })
    return response
if __name__ == "__main__":
    print("Starting Python Flask...")
    app.run(debug=True)





import numpy as np
import pickle
import json

__types = None
__features_with_location = None
__data_columns = None
__model = None





def get_type_names():
    return __types


def get_features_with_location_names():
    return __features_with_location


def load_saved_artifact():
    global __data_columns
    global __types
    global __features_with_location
    global __model

    print("Loading saved property sales artifact...")

    with open("artifact/propertysales_columns.json", "r") as f:
        data = json.load(f)
        __data_columns = data['data_columns']
        __types = [column.replace('type_', '') for column in __data_columns if column.startswith('type_')]
        __features_with_location = [column.replace('location_', '') for column in __data_columns if
                                    column.startswith('location_')]
        # __features_with_location = data['features_with_location']

    with open("artifact/propertysales.pickle", "rb") as f:
        __model = pickle.load(f)

    print("Artifact loaded.")
#
if __name__ == "__main__":
    load_saved_artifact()

    # Now you can test your functions here
    types = get_type_names()
    features_with_location = get_features_with_location_names()

    print("Property Types:", types)
    print("Features with Location:", features_with_location)


