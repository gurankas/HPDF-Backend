from src import app
from flask import jsonify
import requests
from flask import Flask, render_template, json, request, redirect, make_response, Response
from wit import Wit
import sys

app = Flask(__name__)

@app.route("/")
def home():
    return "Hasura Hello World"

# Uncomment to add a new URL at /new

@app.route("/json")
def json_message():
    return jsonify(message="Hello World")

@app.route('/authors', methods=['POST', 'GET'])
def authors():
    users_uri = 'https://jsonplaceholder.typicode.com/users'
    posts_uri = 'https://jsonplaceholder.typicode.com/posts'

    try:
        users_raw_data = requests.get(users_uri)
        posts_raw_data = requests.get(posts_uri)
    except requests.ConnectionError:
        return "Connection error"

# convert JSON data for python
    users_data = users_raw_data.text
    data = json.loads(users_data)

    posts_data = posts_raw_data.text
    data1 = json.loads(posts_data)

# finding author ID from JSON data retrieved
    author_id = []
    for i in range(len(data)):
        author_id.append(data[i]["id"])

# instantiating number of posts by each author in the list to 0
    number_of_posts = {}
    for k in range(len(author_id)):
        number_of_posts[k] = 0

# find number of posts of each author
    for posts in range(len(data)):
        for j in range(len(data1)):
            if data1[j]["userId"] == author_id[i]:
                number_of_posts[posts] += 1

# find names of authors corresponding to their IDs to be sent to the template for displaying
    author_name = []
    for i in range(len(data)):
        author_name.append(data[i]["name"])

    return render_template("authors.html", number_of_posts=number_of_posts, author_name=author_name)

@app.route('/setcookie', methods = ['GET','POST'])
def setcookie():
    return render_template('setcookie.html')

@app.route('/submitted', methods = ['POST'])
def submitted():
    name = request.form['name']
    age = request.form['age']
    resp = make_response(render_template("submitted.html"))
    resp.set_cookie('name', name)
    resp.set_cookie('age', age)
    return resp


@app.route('/getcookie', methods = ['GET', 'POST'])
def getcookie():
    name = request.cookies.get('name')
    age = request.cookies.get('age')
    return render_template('getcookie.html', name=name, age=age)

@app.route('/robots.txt')
def deny():
    return redirect('http://httpbin.org/deny')

@app.route('/image')
@app.route('/html')
def html():
    return render_template('html.html')

@app.route('/input', methods = ['POST', 'GET'])
def input():
    return render_template('form.html')

@app.route('/output', methods=['POST'])
def output():
    weakness = request.form['weakness']
    print(weakness)
    return render_template('output.html', weakness=weakness)


BASE_TOKEN = "FT2YCL5E3TAYDN7KURQUSXILWBQF2E4N"
@app.route('/backend', methods=['POST', 'GET'])
def backend():
    client = Wit(BASE_TOKEN)
    data = request.get_json()
    query = data.get("query")
    resp = client.message(query)
    return jsonify(resp)

ZOMATO_TOKEN = "a82df527166f0a92f9163d7412377025"
@app.route('/zomato', methods=['POST', 'GET'])
def zomato():
    client = Wit(BASE_TOKEN)
    data = request.get_json()
    query = data.get("query")
    resp = client.message(query)
    entities = resp.get("entities")
    location = entities.get("location")[0].get("value")

    z = Zomato(ZOMATO_TOKEN)
    zomato_locations = z.location.get_locations(location)
    entity_type = zomato_locations.raw_data.get("location_suggestions")[0].get("entity_type")
    city_id = zomato_locations.raw_data.get("location_suggestions")[0].get("city_id")
    zomato_suggestions = z.location.get_location_details(city_id,entity_type)
    return jsonify(zomato_suggestions.raw_data)

if __name__ == "__main__":
    app.run(debug=True)
