# Dating-App-Task
Here I am adding the code to design a new feature which can help users to get their potential match based on hobbies for a dating app.
# import the flask framework and its jsonify function which returns a response object

from flask import Flask, jsonify

app = Flask(__name__)
# we can take data from the database but here we created a Sample user data
users = [
    {
        "id": 2,
        "name": "Pari Singh",
        "hobbies": [
            "Music",
            "Cooking",
            "Reading"
        ]
    },
    {
        "id": 3,
        "name": "Naina Patel",
        "hobbies": [
            "Music",
            "Chess",
            "Dance"
        ]
    },
    {
        "id": 4,
        "name": "Amy Bhatt",
        "hobbies": [
            "Cooking"
        ]
    }
]


@app.route('/match/<int:user_id>', methods=['GET'])
# Here a function is created to fetch user hobbies based on user_id and to calculate compatibility based on common hobbies
def get_potential_matches(user_id):
    user_hobbies = [user['hobbies'] for user in users if user['id'] == user_id]
    if not user_hobbies:
        return jsonify({"error": "User not found."}), 404
        
        
    potential_matches = []
    for user in users:
        if user['id'] != user_id:
            common_hobbies = set(user['hobbies']).intersection(set(user_hobbies[0]))
            potential_matches.append({
                "id": user['id'],
                "name": user['name'],
                "hobbies": list(common_hobbies)
            })

   # Sort potential matches based on the number of common hobbies
    potential_matches.sort(key=lambda x: len(x['hobbies']), reverse=True)

    return jsonify(potential_matches)


if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
