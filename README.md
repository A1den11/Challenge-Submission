# Challenge-Submission
import random
from datetime import datetime, timedelta

# Mock Citizen Data (This would be from a government database)
citizens = [
    {"citizen_id": 1, "name": "Alice", "age": 28, "health_conditions": ["None"], "last_taxes_filed": datetime(2022, 4, 15), "vaccination_status": "Up to date"},
    {"citizen_id": 2, "name": "Bob", "age": 60, "health_conditions": ["Diabetes"], "last_taxes_filed": datetime(2022, 4, 1), "vaccination_status": "Not up to date"},
    {"citizen_id": 3, "name": "Charlie", "age": 35, "health_conditions": ["Asthma"], "last_taxes_filed": datetime(2023, 2, 10), "vaccination_status": "Up to date"},
    # Add more citizen data as needed
]

# Example services available
available_services = [
    {"service_id": 1, "service_name": "Tax Filing Reminder", "service_type": "Tax", "age_group": "All", "health_condition": "None", "frequency": "Annually"},
    {"service_id": 2, "service_name": "Health Checkup", "service_type": "Health", "age_group": "60+", "health_condition": "Diabetes", "frequency": "Annually"},
    {"service_id": 3, "service_name": "Vaccination Reminder", "service_type": "Health", "age_group": "All", "health_condition": "None", "frequency": "Every 2 Years"},
    {"service_id": 4, "service_name": "Renew Driver's License", "service_type": "Admin", "age_group": "18+", "health_condition": "None", "frequency": "Every 5 Years"},
    # More services can be added
]
def recommend_services(citizen, services):
    recommendations = []

    # Check age group
    if citizen["age"] >= 60:
        recommendations.append({"service_name": "Health Checkup", "reason": "Age 60+"})
    
    # Check health condition
    if "Diabetes" in citizen["health_conditions"]:
        recommendations.append({"service_name": "Health Checkup", "reason": "Diabetes"})

    # Check if they need to file taxes
    last_tax_date = citizen["last_taxes_filed"]
    if (datetime.now() - last_tax_date).days > 365:  # If it's been more than a year
        recommendations.append({"service_name": "Tax Filing Reminder", "reason": "File taxes"})

    # Check if vaccination status is up-to-date
    if citizen["vaccination_status"] == "Not up to date":
        recommendations.append({"service_name": "Vaccination Reminder", "reason": "Vaccination overdue"})

    # Add generic recommendations based on the profile
    for service in services:
        if service["age_group"] == "All" and service["health_condition"] == "None":
            recommendations.append({"service_name": service["service_name"], "reason": "General service"})

    return recommendations

# Example: Recommend services for each citizen
for citizen in citizens:
    print(f"\nRecommendations for {citizen['name']}:")
    services_for_citizen = recommend_services(citizen, available_services)
    if services_for_citizen:
        for service in services_for_citizen:
            print(f" - {service['service_name']} ({service['reason']})")
    else:
        print(" - No recommendations at this time.")
def send_notification(citizen, service):
    # This is a simplified notification function.
    # In reality, you'd integrate with an email/SMS system.
    print(f"Sending notification to {citizen['name']} for: {service['service_name']} ({service['reason']})")

# Example of sending proactive notifications
for citizen in citizens:
    services_for_citizen = recommend_services(citizen, available_services)
    for service in services_for_citizen:
        send_notification(citizen, service)

        import random
import numpy as np
from sklearn.ensemble import RandomForestClassifier
from datetime import datetime, timedelta

# Sample Data (typically, this would come from various government systems)
citizens = [
    {"citizen_id": 1, "name": "Alice", "age": 28, "last_taxes_filed": datetime(2022, 4, 15), "vaccination_status": "Up to date", "recent_job_change": False},
    {"citizen_id": 2, "name": "Bob", "age": 60, "last_taxes_filed": datetime(2022, 4, 1), "vaccination_status": "Not up to date", "recent_job_change": False},
    {"citizen_id": 3, "name": "Charlie", "age": 35, "last_taxes_filed": datetime(2023, 2, 10), "vaccination_status": "Up to date", "recent_job_change": True},
    # More citizens...
]

# Service Prediction based on Machine Learning (simplified)
def train_predictive_model(citizens):
    # Features: [age, time_since_last_tax, vaccination status, recent_job_change]
    # Labels: [will_need_service_in_3_months] (0 = no, 1 = yes)
    
    data = []
    labels = []
    
    for citizen in citizens:
        time_since_last_tax = (datetime.now() - citizen["last_taxes_filed"]).days
        vaccination_status = 1 if citizen["vaccination_status"] == "Up to date" else 0
        recent_job_change = 1 if citizen["recent_job_change"] else 0
        
        # Assuming simple logic for predicting service needs (could be more advanced)
        will_need_service = 1 if time_since_last_tax > 365 or vaccination_status == 0 else 0
        
        data.append([citizen["age"], time_since_last_tax, vaccination_status, recent_job_change])
        labels.append(will_need_service)
    
    # Training a Random Forest Classifier for simplicity
    model = RandomForestClassifier(n_estimators=100)
    model.fit(data, labels)
    
    return model

# Train the model
model = train_predictive_model(citizens)

# Predict if a citizen will need a service in the next 3 months
def predict_service_need(citizen, model):
    time_since_last_tax = (datetime.now() - citizen["last_taxes_filed"]).days
    vaccination_status = 1 if citizen["vaccination_status"] == "Up to date" else 0
    recent_job_change = 1 if citizen["recent_job_change"] else 0
    
    prediction = model.predict([[citizen["age"], time_since_last_tax, vaccination_status, recent_job_change]])
    return prediction[0] == 1

# Example: Predict service needs for all citizens
for citizen in citizens:
    if predict_service_need(citizen, model):
        print(f"{citizen['name']} may need a service in the next 3 months.")
    else:
        print(f"{citizen['name']} is not predicted to need a service soon.")
import smtplib
from email.mime.text import MIMEText

# Simulate sending email (can be integrated with SMTP in real-world)
def send_email_notification(citizen, service_name, reason):
    # Setting up an example email server (replace with actual SMTP server details)
    from_email = "gov.services@example.com"
    to_email = f"{citizen['name'].lower()}@example.com"  # Simplified email logic
    subject = f"Important: {service_name} Needed"
    body = f"Dear {citizen['name']},\n\nYou are recommended to avail the following service: {service_name}.\nReason: {reason}\n\nPlease act accordingly."
    
    msg = MIMEText(body)
    msg['Subject'] = subject
    msg['From'] = from_email
    msg['To'] = to_email
    
    # Example: Send email (replace with actual email sending logic)
    print(f"Sending email to {to_email}...")
    # Uncomment below for actual email sending (make sure to configure SMTP details)
    # with smtplib.SMTP('smtp.example.com') as server:
    #     server.login("username", "password")
    #     server.sendmail(from_email, to_email, msg.as_string())
    
    print(f"Email sent to {to_email}")

# Sending proactive notifications based on predictions
def send_proactive_notifications(citizens, model):
    for citizen in citizens:
        if predict_service_need(citizen, model):
            # Here, we choose a service based on the prediction
            service_name = "Health Checkup"
            reason = "Due for a health checkup based on recent data."
            send_email_notification(citizen, service_name, reason)
        else:
            print(f"{citizen['name']} does not need proactive services right now.")

# Example: Sending notifications to citizens
send_proactive_notifications(citizens, model)
def virtual_assistant(citizen):
    print(f"Hello {citizen['name']}, how can I assist you today?")
    
    # Example of a simple interaction
    user_input = input("You: ").lower()
    
    if "tax" in user_input:
        if (datetime.now() - citizen["last_taxes_filed"]).days > 365:
            print("It looks like you need to file your taxes. Would you like assistance with that?")
        else:
            print("You're up to date with your tax filings!")
    
    elif "health" in user_input:
        if citizen["vaccination_status"] == "Not up to date":
            print("It seems like you're due for a vaccination. Would you like more information?")
        else:
            print("Your vaccination status is up to date!")
    
    else:
        print("Sorry, I didn't understand. Can you please ask about taxes, health, or other services?")

# Example: Let a citizen interact with the virtual assistant
citizen = citizens[0]  # Pick a citizen
virtual_assistant(citizen)

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Government Digital Services</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <header>
            <h1>Welcome to Government Digital Services</h1>
            <p>Your personalized services, just a click away!</p>
        </header>

        <section class="user-info">
            <h2>Login</h2>
            <form id="login-form">
                <label for="user-id">Enter Your User ID:</label>
                <input type="text" id="user-id" name="user-id" placeholder="Enter User ID" required>
                <button type="submit">Get Services</button>
            </form>
        </section>

        <section id="recommendations" class="hidden">
            <h2>Your Recommended Services</h2>
            <div id="services-list">
                <!-- Personalized services will be displayed here -->
            </div>
        </section>

        <footer>
            <p>&copy; 2024 Government Digital Services | All Rights Reserved</p>
        </footer>
    </div>

    <script src="scripts.js"></script>
</body>
</html>

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    background-color: #f7f7f7;
    color: #333;
}

.container {
    width: 80%;
    max-width: 900px;
    margin: 0 auto;
    padding: 20px;
}

header {
    text-align: center;
    margin-bottom: 40px;
}

h1 {
    font-size: 2.5em;
    color: #2c3e50;
}

h2 {
    font-size: 1.8em;
    color: #34495e;
    margin-bottom: 20px;
}

form {
    display: flex;
    flex-direction: column;
    align-items: center;
}

form label {
    font-size: 1.2em;
    margin-bottom: 10px;
}

form input {
    padding: 10px;
    margin-bottom: 15px;
    width: 80%;
    max-width: 400px;
    font-size: 1em;
    border: 1px solid #ccc;
    border-radius: 4px;
}

form button {
    padding: 12px 25px;
    background-color: #27ae60;
    color: white;
    border: none;
    border-radius: 4px;
    font-size: 1.1em;
    cursor: pointer;
}

form button:hover {
    background-color: #2ecc71;
}

.hidden {
    display: none;
}

#services-list {
    padding: 20px;
    background-color: #ecf0f1;
    border-radius: 5px;
}

.service-item {
    background-color: #fff;
    padding: 10px;
    margin-bottom: 10px;
    border-radius: 4px;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

footer {
    text-align: center;
    margin-top: 50px;
    font-size: 0.9em;
    color: #7f8c8d;
}
document.getElementById('login-form').addEventListener('submit', function(event) {
    event.preventDefault();  // Prevent form submission
    
    const userId = document.getElementById('user-id').value;
    
    if (!userId) {
        alert("Please enter a User ID.");
        return;
    }

    // Send user ID to the Flask back-end
    fetch('/personalized_service', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({ user_id: userId })
    })
    .then(response => response.json())
    .then(data => {
        // Handle the response from the server
        const recommendationsSection = document.getElementById('recommendations');
        const servicesList = document.getElementById('services-list');
        
        // Clear previous recommendations
        servicesList.innerHTML = '';

        if (data.error) {
            servicesList.innerHTML = `<p>${data.error}</p>`;
        } else {
            // Display the personalized message and recommendations
            servicesList.innerHTML = `<p>${data.message}</p>`;
            
            data.recommended_services.forEach(service => {
                const serviceItem = document.createElement('div');
                serviceItem.classList.add('service-item');
                serviceItem.textContent = service;
                servicesList.appendChild(serviceItem);
            });

            // Show recommendations section
            recommendationsSection.classList.remove('hidden');
        }
    })
    .catch(error => {
        console.error("Error:", error);
        alert("Something went wrong, please try again later.");
    });
});



