from flask import Flask, render_template, request, jsonify
from flask_sqlalchemy import SQLAlchemy
from datetime import datetime
app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///water_usage.db'
db = SQLAlchemy(app)
class WaterUsage(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    timestamp = db.Column(db.DateTime, default=datetime.utcnow)
    amount = db.Column(db.Float)
db.create_all()
def receive_water_data():
    data = request.get_json()
    if 'amount' in data:
        new_water_usage = WaterUsage(amount=data['amount'])
        db.session.add(new_water_usage)
        db.session.commit()
        return jsonify({"status": "success"})
    else:
        return jsonify({"status": "error", "message": "Invalid data format"}), 400
def get_water_data():
    water_data = WaterUsage.query.all()
    data = [{"timestamp": entry.timestamp, "amount": entry.amount} for entry in water_data]
    return jsonify(data)
def home():
    return render_template('index.html')
if __name__ == '__main__':
    app.run(debug=True)
